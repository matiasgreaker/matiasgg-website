---
title: "MUSIC"
draft: false
mathjax: true
weight: 4
---

### MUSIC - (Multiple Signal Classification)

Before going into the details about MUSIC I want to revisit some important topics. As I believe these topics lies in the hearth of MUSIC. 
And a note about the math functions: when defining DSP functions, there are often multiple ways of defining them, e.g. for the DFT, some divide by the number of samples, some by the square root of the number of sample, some define it as an infinite sum, etc. These things are of course just details, and the main idea is the same for all of them. But, these details can be annoying if you want to program and test the theory against real examples. As many of these details makes you end up with a different scaling of your data. And I at least then always end up wondering: did I do it correctly or is it **just** a scaling that I'm missing? I will try to specify all of these details. With the idea that you can program everything in python and get **exactly** the same result. 
