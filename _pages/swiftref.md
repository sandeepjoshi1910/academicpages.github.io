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

## Protocols

Protocol is a blueprint of **methods**, **properties** that are related to a particular task or a functionality.

Protocols can be adopted by
1) Classes
2) Structures
3) enums

Any type that satisfies the requirements is said to **conform** to that protocol.

Non conformance of the protocol after any of the above types adopt it generates a **Compiler Error**

Type mothods and properties have to be prefixed with `static` keyword when defined in a protocol even when implemented they'll be replaced by `class` or `static` keyword.

### Syntax

```swift
protocol ExampleProtocol {
    func method_one()
}

class Test:Object, ExampleProtocol {
    func method_one() {
        return 0
    }
}
```

### Property Requirements

* Protocol can require an instance property or a type property
* Requires 1) Name 2) Type
* Protocol specifies whether the property should be 1) gettable 2) gettable and settable

```swift
protocol Person {
    var firstName : String {get set}
    var age : Int {get}
}
```

### Method Requirements

* Methods can be instance methods or type methods
* Variadic parameters are allowed but default parameters are not allowed in protocol definition
* Type methods require a `static` prefix when defined in a protocol

```swift

protocol Color {
    func rgb(r : Int, g : Int, b : Int) -> UIColor
}

```


