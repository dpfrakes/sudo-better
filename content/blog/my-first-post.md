---
title: "My First Post"
date: 2022-04-28T23:23:57-04:00
draft: true
tags: ["blog", "python"]
---

# Something

Here's a thing

```py
# Anagram solution
def anagram_solver(terms):
    dictionary = {}
    for term in terms:
        t = ''.join(sorted(term))
        if t in dictionary:
            dictionary[t].append(term)
        else:
            dictionary[t] = [term]
    return list(dictionary.values())
```

Tada!
