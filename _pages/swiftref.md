---
permalink: /swiftref/
title: "Swift Reference"
author_profile: true

---


## The Basics

#### Constant & Variables

Constants are declared by `let` and variables by `var`. Type is not necessary while declaring them but can also be specified.

``` swift
let numOfHorses = 20
let pi = 3.14
var numDataPoints = 20

var noOfPagesLeft : Int = 35
var currentSwiftVersion : float = 4.1
```

Multiple variables of same type can be defined in a single line

``` swift
var noOfChars, noWords, noOfSentences: Int
```

#### Integers

Swift has signed and unsigned integers in 8 to 64 bit forms. `.min` and `.max` gives minimum and maximum values for various forms of integers. Consistent use of `int` or any other type helps in avoiding unnecessary type conversions and helps in code interoperability. `UInt` is `UInt32` on 32 bit platform and `UInt64` on a 64 bit platform.

In floating Point Integers, `Float` represents 32 bit floating point number and `Double` represents 64 bit floating point number.

#### Type Inference

Swift automatically determines the type of the variable/constant by processing the value or the expression assigned to it without the user explicitly specifying the type. This is helpful while declaring some constants.


