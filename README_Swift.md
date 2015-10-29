

# Backelite - Swift styleguide

* [Ray Wenderlich swift-style-guide](https://github.com/raywenderlich/swift-style-guide)
* [Github swift-style-guide](https://github.com/github/swift-style-guide)
* [Apple The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/index.html)
* [Apple Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html) 

## Table of contents
- [Naming](#naming)
- [Class Prefixes](#class-prefixes)
- [Spacing](#spacing)
- [Comments](#comments)
- Classes and Structures](#Classes-and-structures)
- + [Use of Self](#use-of-self)
- + [Function Declarations](#function-declarations)
- + [Closure Expressions](#closure-expressions)
- + [Constants](#constants)
- + [Optionals](#optionals)
- + [Struct Initializers](#struct-initializers)
- + [Type Inference](#type-inference)
- + [Syntactic Sugar](#syntactic-sugar)
- + [Control Flow](#control-flow)
- + [Semicolons](#semicolons)
- + [Language](#language)


## Naming

Use descriptive names with camel case for classes, methods, variables, etc. Class names should be capitalized, while method names, constants and variables should start with a lower case letter.

**Preferred**
```
let maximumWidgetCount = 100
class WidgetContainer {
  var widgetButton: UIButton
  let widgetHeightPercentage = 0.85
}
```


**Not preferred**
```
let MAX_WIDGET_COUNT = 100
class app_widgetContainer {
  var wBut: UIButton
  let wHeightPct = 0.85
}
```

For functions and init methods, prefer named parameters for all arguments unless the context is very clear.
Include external parameter names if it makes function calls more readable.

```
 func dateFromString(dateString: NSString) -> NSDate
 func convertPointAt(#column: Int, #row: Int) -> CGPoint
 func timedAction(#delay: NSTimeInterval, perform action: SKAction) -> SKAction!
 // would be called like this:
 dateFromString("2014-03-14")
 convertPointAt(column: 42, row: 13)
 timedAction(delay: 1.0, perform: someOtherAction)
```

For methods, follow the standard Apple convention of referring to the first parameter in the method name :

```
class Guideline {
  func combineWithString(incoming: String, options: Dictionary?) { ... }
  func upvoteBy(amount: Int) { ... }
}
```

When referring to functions in prose (tutorials, books, comments) include the required parameter names from the caller's perspective. If the context is clear and the exact signature is not important, you can use just the method name.

```
Call convertPointAt(column:row:) from your own init implementation.
If you implement timedAction, remember to provide an appropriate delay value.
You shouldn't call the data source method tableView(_:cellForRowAtIndexPath:) directly.
```

## Class Prefixes


Swift types are all automatically namespaced by the module that contains them. As a result, prefixes are not required in order to minimize naming collisions. If two names from different modules collide you can disambiguate by prefixing the type name with the module name:

**Not preferred**
```
import MyModule
var myClass = MyModule.MyClass()
```

You should not add prefixes to your Swift types.
If you need to expose a Swift type for use within Objective-C you can provide a suitable prefix (following our Objective-C style guide) as follows:

**Preferred**
```
@objc (RWTChicken) class Chicken {
	   ...
}
```

## Spacing

- Indent using 4 spaces rather than tabs to conserve space and help prevent line wrapping. Be sure to set this preference in Xcode as shown below:
- Method braces and other braces (if/else/switch/while etc.) always open on the same line as the statement but close on a new line.
- Tip: You can re-indent by selecting some code (or A to select all) and then Control-I (or Editor\Structure\Re-Indent in the menu). Some of the Xcode template code will have 4-space tabs hard coded, so this is a good way to fix that.


**Preferred**
```
if user.isHappy {
  //Do something
} else {
  //Do something else
}
```

**Not Preferred**
```
if user.isHappy
{
    //Do something
}
else {
    //Do something else
}
```

- There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a method often means you should refactor into several methods.


## Comments

When they are needed, use comments to explain why a particular piece of code does something. Comments must be kept up-to-date or deleted.
Avoid block comments inline with code, as the code should be as self-documenting as possible. Exception: This does not apply to those comments used to generate documentation.

## Classes and Structures
### Which one to use?
- Remember, structs have value semantics. 
- Use structs for things that do not have an identity. 
- An array that contains [a, b, c] is really the same as another array that contains [a, b, c] and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.
- Classes have reference semantics. 
- Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity.

Sometimes, things should be structs but need to conform to AnyObject or are historically modeled as classes already (NSDate, NSSet). Try to follow these guidelines as closely as possible.

### Example definition
Here's an example of a well-styled class definition:

```
class Circle: Shape {
  var x: Int, y: Int
  var radius: Double
  var diameter: Double {
    get {
      return radius * 2
	} set {
      radius = newValue / 2
    }
  }
  init(x: Int, y: Int, radius: Double) {
   self.x = x
   self.y = y
   self.radius = radius
  }
  convenience init(x: Int, y: Int, diameter: Double) {
   self.init(x: x, y: y, radius: diameter / 2)
  }
  func describe() -> String {
   return "I am a circle at \(centerString()) with an area of \(computeArea())"
  }
  override func computeArea() -> Double {
   return M_PI * radius * radius
  }
  private func centerString() -> String {
   return "(\(x),\(y))"
  } 
}
```

The example above demonstrates the following style guidelines:

- Specify types for properties, variables, constants, argument declarations and other statements with a space after the colon but not before, e.g. x: Int, and Circle: Shape.
- Define multiple variables and structures on a single line if they share a common purpose / context.
- Indent getter and setter definitions and property observers.
- Don't add modifiers such as internal when they're already the default. Similarly, don't repeat the access modifier when overriding a method.


###Use of Self
For conciseness, avoid using self since Swift does not require it to access an object's properties or invoke its methods.
Use self when required to differentiate between property names and arguments in initializers, and when referencing properties in closure
expressions (as required by the compiler):

```
class BoardLocation {
  let row: Int, column: Int
  init(row: Int,column: Int) {
    self.row = row
    self.column = column
    let closure = { () -> () in
      println(self.row)
     } 
  }
}
```

###Function Declarations
Keep short function declarations on one line including the opening brace:

```
func reticulateSplines(spline: [Double]) -> Bool {
  // reticulate code goes here
}
```

For functions with long signatures, add line breaks at appropriate points and add an extra indent on subsequent lines:

```
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
    translateConstant: Int, comment: String) -> Bool {
  // reticulate code goes here
}
```

###Closure Expressions
Use trailing closure syntax wherever possible. In all cases, give the closure parameters descriptive names:

```
return SKAction.customActionWithDuration(effect.duration) { node, elapsedTime in
  // more code goes here
}
```

For single-expression closures where the context is clear, use implicit returns:
```
attendeeList.sort { a, b in 
	a> b
}
```

Use shorthand arguments ($0, $1, $2 and so on) only when arguments have an order and belong to the same type :
```
reversed = sort(names, { $0 > $1 } )
```

###Types
Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.


**Preferred**
```
let width = 120.0                                    //Double
let widthString = (width as NSNumber).stringValue    //String
```

**Not Preferred**
```
let width: NSNumber = 120.0                                 //NSNumber
let widthString: NSString = width.stringValue               //NSString
```

In Sprite Kit code, use CGFloat if it makes the code more succinct by avoiding too many conversions.

###Constants
Constants are defined using the **let** keyword, and variables with the **var** keyword. Any value that **is** a constant mustbe defined appropriately, using the **let** keyword. As a result, you will likely find yourself using let far more than var.
**Tip**: One technique that might help meet this standard is to define everything as a constant and only change it to a variable when the compiler
complains!

###Optionals

Declare variables and function return types as optional with ? where a nil value is acceptable.
Use implicitly unwrapped types declared with ! only for instance variables that you know will be initialized later before use, such as subviews that will be set up in viewDidLoad.

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain:

```
self.textContainer?.textLabel?.setNeedsDisplay()
```

Use optional binding when it's more convenient to unwrap once and perform multiple operations:
```
if let textContainer = self.textContainer {
  // do many things with textContainer
}
```

When naming optional variables and properties, avoid naming them like optionalString or maybeView since their optional-ness is already in the type declaration.
For optional binding, shadow the original name when appropriate rather than using names like unwrappedView or actualLabel.

**Preferred**
```
var subview: UIView?
// later on...
if let subview = subview {
  // do something with unwrapped subview
}
```

**Not Preferred**
```
var optionalSubview: UIView?
if let unwrappedSubview = optionalSubview {
  // do something with unwrappedSubview
}
```

###Struct Initializers
Use the native Swift struct initializers rather than the legacy CGGeometry constructors.

**Preferred**
```
let bounds = CGRect(x: 40, y: 20, width: 120, height: 80)
var centerPoint = CGPoint(x: 96, y: 42)
```

**Not Preferred**
```
let bounds = CGRectMake(40, 20, 120, 80)
var centerPoint = CGPointMake(96, 42)
```

Prefer the struct-scope constants CGRect.infiniteRect, CGRect.nullRect, etc. over global constants CGRectInfinite, CGRectNull, etc. For existing variables, you can use the shorter .zeroRect.


###Type Inference
The Swift compiler is able to infer the type of variables and constants. You can provide an explicit type via a type alias (which is indicated by the type after the colon), but in the majority of cases this is not necessary.
Prefer compact code and let the compiler infer the type for a constant or variable.

**Preferred**
```
let message = "Click the button"
var currentBounds = computeViewBounds()
```

**Not Preferred**
```
let message: String = "Click the button"
var currentBounds: CGRect = computeViewBounds()
```

**NOTE**: Following this guideline means picking descriptive names is even more important than before.

###Syntactic Sugar
Prefer the shortcut versions of type declarations over the full generics syntax.

**Preferred**
```
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**Not Preferred**
```
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

###Control Flow
Prefer the for-in style of for loop over the for-condition-increment style.

**Preferred**
```
for _ in 0..<3 {
  println("Hello three times")
}
for (index, person) in enumerate(attendeeList) {
  println("\(person) is at position #\(index)")
}
```

**Not Preferred**
```
for var i = 0; i < 3; i++ {
  println("Hello three times")
}
for var i = 0; i < attendeeList.count; i++ {
  let person = attendeeList[i]
  println("\(person) is at position #\(i)")
}
```

###Semicolons
Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.
Do not write multiple statements on a single line separated with semicolons.
The only exception to this rule is the for-conditional-increment construct, which requires semicolons. However, alternative for-in constr ucts should be used where possible.

**Preferred**
```
var swift = "not a scripting language"
```

**Not Preferred**
```
var swift = "not a scripting language";
```

**NOTE**: Swift is very different to JavaScript, where omitting semicolons is generally considered unsafe

###Language
Use US English spelling to match Apple's API.

**Preferred**
```
var color = "red"```

**Not Preferred**
```
var colour = "red"
```


