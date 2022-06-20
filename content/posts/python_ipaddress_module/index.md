---
title: "Python's ipaddress Module"
date: 2022-06-18T21:59:23-04:00
draft: false
toc: true
images:
tags:
  - python
---
## Introduction
*(some familiarity and experience with Python is assumed for this article)*

Since version 3.3 (released September 2012), Python has included an **ipaddress** module as part of its standard library (that is, it's included as part of any default Python 3.3+ installation). For a network engineer, this can be a remarkably useful and easy way to work with IP addresses and subnets in Python.

## Importing
Since **ipaddress** is part of the standard library, to utilize it, it's as simple as starting a new Python script (or REPL) and doing:

```python
import ipaddress
```

Once imported, this gives you access to a number of different classes that can (for example) test whether a given address falls within a given subnet, or if an IP address is valid IPv4.

## So, how do I use it?

Sticking with IPv4 for now, let's review the three major "classes" you'll likely work with using **ipaddress**. Those three are:
- *IPv4Address*
- *IPv4Interface*
- *IPv4Network*

### IPv4Address

The first, *IPv4Address*, is fairly self-explanatory, and accepts only individual IP addresses (no subnet/CIDR masks):

```python
>>> import ipaddress
>>> ipaddress.IPv4Address("86.75.30.9")
IPv4Address('86.75.30.9')
```

So what good does this do?

Imagine you have a list of data, possibly a list of hostnames and IP addresses, or a list of IP address data of unknown validity. If you want to quickly filter this data out to just valid IP addresses, you can do something as simple as the following example (conveniently, you can get the "plain-text" representation of any **ipaddress** object by calling the *str()* function on it):

```python
sus_ip_list = [
  "86.75.30.9",
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
['86.75.30.9', '8.8.8.8', '172.16.1.1', '192.0.2.42', '198.51.100.42', '203.0.113.42']
```

Additionally, there are some "methods" that allow you to determine what sort of IPv4 address you're dealing with - public, private, multicast, loopback, etc., or format it as a reverse pointer (PTR) DNS record:

```python
>>> ipaddress.IPv4Address("86.75.30.9").is_global
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

*To be continued...*
