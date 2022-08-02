---
title: "Beginner Python Examples"
date: 2022-07-16T22:30:12-04:00
draft: false
toc: true
images:
tags:
  - untagged
---

## Introduction

For anyone new to Python, or looking for a quick refresher on the basics, consider this article an overview of various ways to get started reading, modifying, and working with data in Python.

## Examples

For these first two examples, let's imagine some data, such as a list of MAC addresses. In Python, a *collection* of items is called a *List*. For this particular list of MAC addresses, I'll use a list of *strings*, which is really just a fancy word for some text. Lists are represented via square brackets **[]**, with each item separated by commas, and strings are enclosed in quotes **""** (single or double, your choice):

```python
>>> known_mac_addresses = [
    "00-11-22-33-44-55",
    "01-23-45-67-89-0A",
    "AA-BB-CC-DD-EE-FF",
    "36-56-9A-B4-0E-ED",
    "01-EF-AF-BC-DD-23",
    "86-75-30-9E-19-00",
    "AB-CD-EF-01-23-45",
    "89-46-C3-69-D8-D4",
]
```

If you stare at the list above long enough, you may notice that the last item of the list ends with a comma, even though there isn't another item that comes after it. This is a matter of personal preference, but trailing commas are optional in Python lists (as the above example illustrates, I prefer them myself for readability, and also for ease of any future additions to the list, but mainly because that's what the *Black* formatter does, along with double-quotes for strings).

Now that some data has been established, let's look at some examples of what we can do with it.

### Example 1 - Find a Single Item

```python
mac_to_find = "86-75-30-9E-19-00"

if mac_to_find in known_mac_addresses:
    print(f"Found {mac_to_find} in list of known MAC addresses!")
else:
    print(f"* Could not find {mac_to_find} in list of known MAC addresses.")
```

Running the above code should produce something like:

```python
... 
Found 86-75-30-9E-19-00 in list of known MAC addresses!
```

### Example 2 - Multiple Items (Looping)

```python
maclist_to_find = [
    "86-75-30-9E-19-00",
    "01-23-45-67-89-0A",
    "FA-CE-B0-0C-FA-CE",
    "89-46-C3-69-D8-D4",
]

for mac_address in maclist_to_find:
    if mac_address in known_mac_addresses:
        print(f"Found {mac_address} in list of known MAC addresses!")
    else:
        print(f"* Could not find {mac_address} in list of known MAC addresses.")
```

**WORK IN PROGRESS**
