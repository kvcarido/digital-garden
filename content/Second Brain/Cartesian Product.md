---
title: Cartesian Product
draft: false
tags:
  - seed
---
> Mentioned during a lecture of the [Missing Semester of CS](http://missing.csail.mit.edu) when explaining advanced methods of [[shell#Shell Globbing|shell globbing]] with curly brackets `{}` 

A mathematical concept that combines every element of one set with every element of another set, resulting in a new set of pairs or tuples representing all possible combinations between the two sets.

```python
set_a = {1, 2}
set_b = {x, y}

# Cartesian product
{
	 [1, x],
	 [1, y],
	 [2, x],
	 [2, y]
 }
```
