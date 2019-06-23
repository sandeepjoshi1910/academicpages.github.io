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


## Optionals

Optionals are used when it is unknown whether a value of the specific type will be set to the variable.

`Int?` is read as Int Optional and it can hold `nil` and `Int` values.

A declaration `var currentVelocity: Float?` instantly stores a `nil` to the variable. So `nil` can't be used with any other constants or variables which are not optional as `nil` represents absence of a value and that can happen only with optional types.

Optional types of `Any` can be set to `nil` and not just Object types. This includes all objects and enums, functions etc.

Using `!` an optional value can be unwrapped. For ex. `var someStr : String? = "example"` can be force unwrapped as `print(someStr!)` to extract the value. We do this when we are sure the optional variable consists the value.

### Optional Binding

This is used to unwrap an optional variable and make the value available in a temporary variable else let us handle the case when the variable has `nil`. This eliminated the use of `!` or force unwrapping of the optional variable.

``` swift

if let someUnwrappedVal = optionalVar {
    // optionalVar has some value and is available in const someUnwrappedVal
} else {
    // Handle the case when optionalVar is nil
}

if var someUnwrappedVal = optionalVar {
    // optionalVar has some value and is available in var someUnwrappedVal
} else {
    // Handle the case when optionalVar is nil
}

```

## Strings

#### Basic Syntax

``` swift
let someStr = "A String"

let mlineStr = """
line 1
line2
"""
```
Strings are value types. This means strings are passed by value, they are copied and sent to functions/methods.

#### Empty String

`let str = "" var someStr = String()`

To check if a string is empty, 
``` swift
if str.isEmpty {
    // Do something
}
```
#### Iterating over characters in a string

```swift
    for character in "Swift-Language" {
        print(character)
    }
```

#### Constructing a String from characters
``` swift
let chars : [Character] = ['s','w','i','f','t']

let lang = String(chars)
```

#### String and Character concatenation

``` Swift
    let str1 = "Rick"
    let str2 = " and Morty"
    var show = str1 + str2

    show.append('!')
```

#### String count

` string.count `

#### Indexing

``` swift

let str = "Macbook pro"
let stIndex  = str.startIndex
let endIndex = str.endIndex

// Access a character at an index
print(str[stIndex])

// To extract a character at a position
print(str[str.index(stIndex, offsetBy: 3)])


// To extract a character at before position indicated by an index
print(str[str.index(before: endIndex)])


// To extract a character after a position indicated by an index
print(str[str.index(after: stIndex)])
```

#### Inserting and Removing characters

##### Inserting
``` swift

var str = "lorem ipsum"

// Inserting a single character at an index
str.insert(".", at: str.endIndex)

// Inserting a string at an index
str.insert(contentsOf: " script", at: str.index(before: str.endIndex))

```

##### Removing

``` swift

var str = "swift"

str.remove(at: str.index(before: str.startIndex))

// Removing more than one character needs a range of indices and chars at that range will be removed.

let removeRange = str.startIndex..<str.index(str.startIndex,offsetBy:4)

str.removeSubrange(removeRange)

```
> You can use the `insert(_:at:), insert(contentsOf:at:), remove(at:), and removeSubrange(_:)` methods on any type that conforms to the `RangeReplaceableCollection` protocol. This includes String, as shown here, as well as collection types such as Array, Dictionary, and Set.


#### Substrings






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

### Dictionaries

Dictionary is a map between keys of same type and values of same type. Dictionary key *must conform to hashable protocol*. Type of the dictionary can be inferred using type inference.

##### Initializing a dictionary

``` swift
let map : [String:Int] = [:]
let map = [String:Int]()

var charCount : [String:Int] = ["a":1,"b":2,"c":3,"d":4]
```

##### Various operations on dictionary

##### Count
```swift 
dict.count
```

##### Empty Check
``` swift 
dict.isEmpty
```

##### Add Item
``` swift 
dict["a"] = 4 
```

##### Remove Item
``` swift 
dict["a"] = nil 
```

##### Iterating dict
```swift 
for (key,value) in dict {
    print("Key : \(key) and Value: \(value)")
}
```

##### Iterating over Keys

``` swift
for key in dict.keys {
    print(key)
}
```

##### Iterating over values

``` swift
for value in dict.values {
    print(value)
}
```

##### Get an array of keys or values

``` swift
let keyArray = [String](dict.keys)
let valueArray = [Int](dict.values)
```


### Initialization

The process of preparing an instance of class, struct or enum to be ready for use. It involves setting all the class properties and any other initialization before it can be used. Any properties which are not optional have to have either a default inital value while definition of the property or initialized to a value in the init method.

> Swift initializers doesn't return anything unlike Objective C initializers

Initializers need to have argument labels when called. This way swift figures which initializer to call when there is more than one initializer.

## Swift Questions

### 1. When do we use `@objc` and why?

Answer/Notes from this blog post on [SwiftUnboxed](https://swiftunboxed.com/interop/objc-dynamic/) : 
Sometimes when we have to use a function as a selector, we need to declare the func as @objc because,
selector is an Objective C concept. Generally @objc makes the swift code available to Objective C and
the Objective C runtime.

1. @objc makes things visible to Objective C code. Need this for setting up target/action on buttons
    and gesture recognizers.
2. Dynamic opts in for dynamic dispatch. Need this for KVO support
3. Only way to do dynamic dispatch currently is through the Objective C runtime. So need to use
    @objc if you also use dynamic.
    
### 2. What is the difference between awakeFromNib and ViewDidLoad?

AwakeFromNib is called when the nib file is loaded. ViewDidLoad is called when the view is loaded in the viewControllers.
