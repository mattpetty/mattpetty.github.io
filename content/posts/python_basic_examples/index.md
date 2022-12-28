---
title: "Beginner Python Examples for Network Engineers"
date: 2022-07-16T22:30:12-04:00
draft: false
toc: true
images:
tags:
  - python
---

## Introduction

For anyone new to Python, or looking for a quick refresher on the basics, consider this article an overview of various ways to get started reading, modifying, and working with data in Python. Naturally, given the nature of this blog, this post will take a Network Engineering-centric approach in the examples.

## Examples

For these first two examples, let's imagine some data, such as a list of MAC addresses. In Python, a *group of items* is called a **List** (there are also multiple other groupings in Python, but that's a subject for another blog post). For this particular list of MAC addresses, I'll use a list of *strings*, which is really just another word for "text." Lists are represented via square brackets **[]**, with each item separated by commas, and strings are enclosed in quotes **""** (single or double, your choice):

```python
>>> known_mac_addresses = [
    "00-11-22-33-44-55",
    "01-23-45-67-89-0A",
    "AA-BB-CC-DD-EE-FF",
    "36-56-9A-B4-0E-ED",
    "01-EF-AF-BC-DD-23",
    "86-75-30-98-67-53",
    "AB-CD-EF-01-23-45",
    "89-46-C3-69-D8-D4",
]
```

> 📝 If you stare at the list above long enough, you may notice that the last item of the list ends with a comma, even though there isn't another item that comes after it. This is a matter of personal preference, but trailing commas are optional in Python lists (as the above example illustrates, I prefer them myself for readability, and for ease of any future additions to the list, but also mainly because that's what the *[Black](https://github.com/psf/black)* code formatter does, along with double-quotes for strings).

Now that some data has been established, let's look at some examples of what we can do with it.

### Example 1 - Find a Single Item

One of the nicest things about Python is its simple syntax, which can often closely resemble natural language or pseudocode in the sense that it's very easy to read and understand. In particular, it becomes extremely easy to check for the existence of a specific *string of text* inside a larger string (or a *list* of strings, in this example) simply by using the **"in"** keyword:

```python
known_mac_addresses = [
    "00-11-22-33-44-55",
    "01-23-45-67-89-0A",
    "AA-BB-CC-DD-EE-FF",
    "36-56-9A-B4-0E-ED",
    "01-EF-AF-BC-DD-23",
    "86-75-30-98-67-53",
    "AB-CD-EF-01-23-45",
    "89-46-C3-69-D8-D4",
]

mac_to_find = "86-75-30-98-67-53"

if mac_to_find in known_mac_addresses:
    print(f"Found {mac_to_find} in list of known MAC addresses!")
else:
    print(f"* Could not find {mac_to_find} in list of known MAC addresses.")
```

The code above shows a way of specifying an exact match for a given string within a list of strings. Let's focus just on that **if** statement:

```python
if mac_to_find in known_mac_addresses:
```

Running the above code should produce something like:

```python
... 
Found 86-75-30-98-67-53 in list of known MAC addresses!
```

Without having had to write any sort of regular expression or come up with a kludgy method of string matching, this ability is built right in to Python using the **in** keyword.

### Example 2 - Multiple Items (Looping)

Finding a *single* string in a list of strings is great, but what if we have *more than one* string that we want to match? With Python, we can use **for** loops to easily compare multiple values against each other. A **for** loop simply iterates over every item in a list (or any similar group of items in Python).

Consider the following example:

```python
known_mac_addresses = [
    "00-11-22-33-44-55",
    "01-23-45-67-89-0A",
    "AA-BB-CC-DD-EE-FF",
    "36-56-9A-B4-0E-ED",
    "01-EF-AF-BC-DD-23",
    "86-75-30-98-67-53",
    "AB-CD-EF-01-23-45",
    "89-46-C3-69-D8-D4",
]

maclist_to_find = [
    "86-75-30-98-67-53",
    "01-23-45-67-89-0A",
    "C0-FF-EE-C0-FF-EE",
    "89-46-C3-69-D8-D4",
]

for mac_address in maclist_to_find:
    if mac_address in known_mac_addresses:
        print(f"Found {mac_address} in list of known MAC addresses!")
    else:
        print(f"* Could not find {mac_address} in list of known MAC addresses.")
```

Once again, let's focus on a specific line from the above example:

```python
for mac_address in maclist_to_find:
```

By using the **for** loop, the above code will "walk" through the list *maclist_to_find* and compare each individual item in it (that is, each desired MAC address) against the initial list of known MAC addresses.

Running the example code above will produce the following result:

```python
...
Found 86-75-30-98-67-53 in list of known MAC addresses!
Found 01-23-45-67-89-0A in list of known MAC addresses!
* Could not find C0-FF-EE-C0-FF-EE in list of known MAC addresses.
Found 89-46-C3-69-D8-D4 in list of known MAC addresses!
```

### Conclusion

Hopefully, these simple examples show just how easy Python makes it to start working with text-based data (particularly *strings* and *lists*) with a combination of simple syntax, such as **"in"**, and using **for** loops. Automate away!
