---
title: "Python's ipaddress Module"
date: 2022-08-09T22:30:30-04:00
draft: false
toc: true
images:
tags:
  - python, networking
---
## Introduction

> 📝 (Note: some [familiarity](https://mattpetty.github.io/2022/07/16/beginner-python-examples/) with Python is assumed for this article)

Since version 3.3 (released September 2012), Python has included an **ipaddress** module as part of its standard library (that is, it's included as part of any default Python 3.3+ installation). For a network engineer, this can be a remarkably useful and easy way to work with IP addresses and subnets in Python.

The official documentation is [here](https://docs.python.org/3/library/ipaddress.html), but read on for a basic summary of this module and some helpful uses for it.

## Importing

Since **ipaddress** is part of the standard library, to utilize it, it's as simple as starting a new Python script (or REPL) and doing:

```python
import ipaddress
```

Once imported, this gives you access to a number of different classes that can (for example) test whether a given address falls within a given subnet, or if an IP address is valid IPv4.

## So, how do I use it?

Sticking with **IPv4** for now, let's review the three major "classes" you'll likely work with using **ipaddress**. Those three are:

- *IPv4Address*
- *IPv4Network*
- *IPv4Interface*

### IPv4Address

The first, *IPv4Address*, is fairly self-explanatory, and accepts only individual IP addresses (no subnet/CIDR masks):

```python
>>> import ipaddress
>>> ipaddress.IPv4Address("192.0.2.9")
IPv4Address('192.0.2.9')
```

So what good does this do?

First, it allows you to use addition and subtraction to easily increase or decrease the IP address object:

```python
>>> ipaddress.IPv4Address("192.0.2.9")
IPv4Address('192.0.2.9')
>>> ipaddress.IPv4Address("192.0.2.9") + 1
IPv4Address('192.0.2.10')
>>> ipaddress.IPv4Address("192.0.2.9") - 8
IPv4Address('192.0.2.1')
>>> ipaddress.IPv4Address("192.0.2.9") + 33
IPv4Address('192.0.2.42')
>>> ipaddress.IPv4Address("192.0.2.9") + 289
IPv4Address('192.0.3.42')
```

For another use case, imagine you have a list of data, possibly a list of hostnames and IP addresses, or a list of IP address data of unknown validity. If you want to quickly filter this data out to just valid IP addresses, you can do something as simple as the following example (conveniently, you can get the "plain-text" representation of any **ipaddress** object by calling the *str()* function on it):

```python
sus_ip_list = [
  "100.111.222.3",
  "dns.google",
  "8.8.8.8",
  "0.0.0.0/0",
  "172.16.1.1",
  "192.0.2.42",
  "198.51.100.42",
  "203.0.113.42",
  "would you like a toasted tea-cake?",
]

valid_ip_list = []

for maybe_ip in sus_ip_list:
  try:
    valid_ip = ipaddress.IPv4Address(maybe_ip)
    valid_ip_list.append(str(valid_ip))
  except ipaddress.AddressValueError:
    print(f'Error: "{maybe_ip}" is not a valid host IPv4 address')
```

Running the above code produces the following result:

```python
>>> ...
Error: "dns.google" is not a valid host IPv4 address
Error: "0.0.0.0/0" is not a valid host IPv4 address
Error: "would you like a toasted tea-cake?" is not a valid host IPv4 address

>>> valid_ip_list
['100.111.222.3', '8.8.8.8', '172.16.1.1', '192.0.2.42', '198.51.100.42', '203.0.113.42']
```

Additionally, there are some properties that allow you to determine what sort of IPv4 address you're dealing with - public, private, multicast, loopback, etc.; or format it as a reverse pointer (PTR) DNS record:

```python
>>> ipaddress.IPv4Address("8.8.8.8").is_global
True
>>> ipaddress.IPv4Address("172.16.1.1").is_global
False
>>> ipaddress.IPv4Address("172.16.1.1").is_private
True
>>> ipaddress.IPv4Address("127.0.0.1").is_loopback
True
>>> ipaddress.IPv4Address("172.16.1.1").reverse_pointer
'1.1.16.172.in-addr.arpa'
```

### IPv4Network

The next class, *IPv4Network*, differs from *IPv4Address* in that it requires a *subnet* (in CIDR form) as the input value, as opposed to a single IP address. This means that for the given subnet mask you're passing to *IPv4Network*, the corresponding IP address must be a network address (i.e. lowest of the subnet).

```python
>>> ipaddress.IPv4Network("192.0.2.0/24")
IPv4Network('192.0.2.0/24')
```

Try to pass a subnet address that isn't the network address, and an exception will be raised:

```python
>>> ipaddress.IPv4Network("192.0.2.1/24")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.7/ipaddress.py", line 1536, in __init__
    raise ValueError('%s has host bits set' % self)
ValueError: 192.0.2.1/24 has host bits set
```

The *IPv4Network* class has all the same attributes and properties of the *IPv4Address* class, as well as a few more, to get information like the subnet's broadcast address, long-form subnet mask, host (wildcard) mask, etc. (refer to the official docs for more; these are just a few examples):

```python
>>> ipaddress.IPv4Network("192.0.2.0/24").network_address
IPv4Address('192.0.2.0')
>>> ipaddress.IPv4Network("192.0.2.0/24").broadcast_address
IPv4Address('192.0.2.255')
>>> ipaddress.IPv4Network("192.0.2.0/24").netmask
IPv4Address('255.255.255.0')
>>> ipaddress.IPv4Network("192.0.2.0/24").hostmask
IPv4Address('0.0.0.255')
```

(notice that many of the returned addresses are actually IPv4Address objects!)

Also, using *IPv4Address* and *IPv4Network* objects together enables the ability to quickly determine if a given address is part of a given subnet using the **"in"** keyword:

```python
>>> ipaddress.IPv4Address("192.0.2.42") in ipaddress.IPv4Network("192.0.2.0/24")
True
>>> ipaddress.IPv4Address("10.86.75.39") in ipaddress.IPv4Network("10.0.0.0/8")
True
>>> ipaddress.IPv4Address("8.8.8.8") in ipaddress.IPv4Network("172.16.0.0/16")
False
```

### IPv4Interface

This brings us to the *IPv4Interface* class. Think of this almost like a combination of *IPv4Address* and *IPv4Network*; it takes both a host IP address *and* a CIDR mask, a syntax which is frequently seen on *network device interfaces*:

```python
>>> ipaddress.IPv4Interface("192.168.42.10/24")
IPv4Interface('192.168.42.10/24')
```

This could also be used to describe both a *host* on a network and its *subnet mask* with a single object.

Consider the following example: with a standard of using the first usable address in a subnet as the default gateway, all the typically relevant IP-related data (host address, subnet mask, default gateway) could be deduced from a *single* **IPv4Interface** object:

```python
my_host = ipaddress.IPv4Interface("172.16.99.42/24")

host_ip_address = my_host.ip
host_subnet_mask = my_host.netmask
host_gateway = my_host.network.network_address + 1
```

Printing the output from the example above produces the following:

```python
>>> print(f"""
... IP Address:  {host_ip_address}
... Subnet Mask: {host_subnet_mask}
... Gateway:     {host_gateway}
... """)

IP Address:  172.16.99.42
Subnet Mask: 255.255.255.0
Gateway:     172.16.99.1
```

## Summary

I hope these examples have shed some light on how Python's **ipaddress** module can be a useful tool any time you need to analyze, process, or generate IP addressing-related data. It's well-documented and readily available as part of the standard library, making it an easy choice to add to your Python networking toolkit.
