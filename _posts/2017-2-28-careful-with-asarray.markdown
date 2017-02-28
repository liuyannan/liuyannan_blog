---
layout: page
title:  "Be Careful with Numpy Asarray"
categories: Python
tags: Python
date:   2017-2-28 15:47:00 +08:00
---

Remember, be careful with the `Numpy.asarray()` function. `Numpy.asarray()` would not create a copy of the original object, it is already an numpy array. Below is the quote from the official website.

> Returns:	out : ndarray
>
>Array interpretation of a. No copy is performed if the input is already an ndarray. If a is a subclass of ndarray, a base class ndarray is returned.
