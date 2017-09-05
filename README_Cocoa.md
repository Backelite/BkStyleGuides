

# Backelite - Objective-C Coding Style

## Sources

* [About Objective-C](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)
* [Objective-C](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/ObjectiveC.html)
* [Introduction to Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [App Programming Guide for iOS](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa - Apple Naming Methods](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html)
* [Advanced Memory Management Programming Guide - ](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)
* [NYTimes Objective-C Style Guide](https://github.com/NYTimes/objective-c-style-guide)
* [github - objective-c-style-guideC](https://github.com/github/objective-c-style-guide)
* [Raywenderlich - objective-c-style-guideC](https://github.com/raywenderlich/objective-c-style-guide)
* [Google - objcguide](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)

## Table of Contents

* [Language](#language)
* [Spacing](#spacing)
* [Naming](#naming)
  * [Underscores](#underscores)
* [Methods](#methods)
* [Code Organization](#code-organization)
* [Variables](#variables)
* [Literals](#literals)
* [Types](#types)
* [Init](#init)
* [Dot-notation syntax](#dot-notation-syntax)
* [Conditionals](#conditionals)
  * [Ternary operator](#ternary-operator)
  * [Multiple conditionals](#multiple-conditionals)
  * [Yoda style](#yoda-style)
  * [Switch](#switch)
* [Braces](#braces)
* [Constants](#constants)
* [Enumerated types](#enumerated-types)
* [Bitmasks](#bitmasks)
* [Booleans](#booleans)
* [Private properties](#private-properties)
* [Error handling](#error-handling)
* [Try / catch](#try-catch)
* [CGRect functions](#cgrect-functions)
* [Xcode project](#xcode-project)


## Language

US English should be used.

**Preferred :**

```objc
UIColor *myColor = [UIColor whiteColor];
```

**Not preferred :**

```objc
UIColor *myColour = [UIColor whiteColor];
```

## Spacing

* Indent using 4 spaces (this conserves space in print and makes line wrapping less likely). Never indent with tabs as they can be configured in different ways depending on editors and user preferences. Be sure to set this preference in Xcode.


**Preferred:**

```objc
if (user.isHappy) {
    // Do something
} else {
    // Do something else
}
```

**Not preferred: (look closer to spot the difference)**

```objc
if (user.isHappy) {
	// Do something
} else {
	// Do something else
}
```

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good.

**Preferred:**

```objc
UIButton *settingsButton;
```

**Not Preferred:**

```objc
UIButton *setBut;
```

A three letter prefix (for instance `BKT`) should always be used for class names and constants, however may be omitted for Core Data entity names.

Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

**Preferred:**

```objc
static NSTimeInterval const BKTTutorialViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Not Preferred:**

```objc
static NSTimeInterval const fadetime = 1.7;
```

Properties should be camel-case with the leading word being lowercase. Use auto-synthesis for properties rather than manual @synthesize statements unless you have good reason.

**Preferred:**

```objc
@property (strong, nonatomic) NSString *descriptiveVariableName;
```

**Not Preferred:**

```objc
id varnm;
```

### Underscores

When using properties, instance variables should always be accessed and mutated using `self.`. This means that all properties will be visually distinct, as they will all be prefaced with `self.`. 

An exception to this: inside initializers, the backing instance variable (i.e. _variableName) should be used directly to avoid any potential side effects of the getters/setters.

Local variables should not contain underscores.

## Methods

[Apple-NamingMethods](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html)

 * In method signatures, there should be a space after the method type (-/+ symbol). There should be a space between the method segments (matching Apple's style).  Always include a keyword and be descriptive with the word before the argument which describes the argument.

**For instance:**

```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

 * Method should not exceeds 50 lines. This limit allows code readability, reusability and testability.

 * Use pertinent keywords to describe your method and its parameters.


 **Preferred:**
 
 ```objc
 - (void)setExampleText:(NSString *)text image:(UIImage *)image;
 - (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
 - (id)viewWithTag:(NSInteger)tag;
 - (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
 ```

 **Not Preferred:**

 ```objc
 -(void)setT:(NSString *)text i:(UIImage *)image;
 - (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
 - (id)taggedView:(NSInteger)tag;
 - (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
 - (instancetype)initWith:(int)width and:(int)height;  // Never do this.
 ```

 * Only use one `return` in a method to make debugging easier and reduce the use of breakpoints.



## Code Organization

Use `#pragma mark -` to categorize methods in functional groupings and protocol/delegate implementations following this general structure.

```objc
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```


## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants.

[Private properties](#private-properties) should be used in place of instance variables whenever possible. Although using instance variables is a valid way of doing things, by agreeing to prefer properties our code will be more consistent. 

Direct access to instance variables that 'back' properties should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).


**Preferred:**

```objc
@interface BKTSection : NSObject
@property (nonatomic) NSString *headline;
@end
```

**Not preferred:**

```objc
@interface BKTSection : NSObject {
	NSString *headline;
}
```


## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values can not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**Preferred:**

```objc
NSArray *names = @[@"Gilles", @"Philippe", @"David", @"Mickael", @"Gyl", @"Sylvia"];
NSDictionary *productManagers = @{@"iPhone": @"Jerome", @"iPad": @"Kamal", @"Mobile Web": @"Clément"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *zipCode = @75008;
```

**Not Preferred:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Gilles", @"Philippe", @"David", @"Mickael", @"Gyl", @"Sylvia", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Jerome", @"iPhone", @"Kamal", @"iPad", @"Clément", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:75008];
```

## Types

Apple's primitives like `NSInteger`, `NSUInteger`, `CGFloat` should be used instead of `int`, `long`, `float`. This will prevent unusual behaviors due to platform architectures (32 vs 64 bits).


**Preferred:**

```objc
CGRect rect = self.view.frame;
rect.x = 0.0f;
rect.x = 10.0f;
self.view.frame = rect;
```

**Not preferred:**

```objc
CGRect rect = self.view.frame;
rect.x = 0.0;  	// this is a double
rect.x = 1;		// this is an int
self.view.frame = rect;
```

## Init

`init` method should be structured like this :


```objc
- (instancetype)init {
    self = [super init]; // or call the designated initializer
    if (self) {
        // Custom initialization
    }

   return self;
}
```

`NS_DESIGNATED_INITIALIZER` macro should be used to document the init method you want to designate.


```objc
- (instancetype)init NS_DESIGNATED_INITIALIZER;
```

### about the `new` usage

The `new` method should be avoided because it makes unclear which designated initializer method will be used. 

**Preferred:**

```objc
BKTModel *view = [[BKTModel alloc] init];
```

**Not Preferred:**

```objc
BKTModel *view = [BKTModel new];
```

## Dot-Notation Syntax

Dot-notation is **recommanded** for getting and setting properties.
Bracket notation is preferred in other cases.

**Preferred:**

```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not preferred:**

```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Conditionals

* **ALWAYS** use braces !

**Preferred:**

```
if (!error) {
    return success;
}
```

**Not preferred:**

```objc
if (!error) return success;
```

or

```objc
if (!error)
	return success;
```

### Ternary Operator

The Ternary operator, `?:` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an `if` statement, or refactored into instance variables.

**Preferred:**

```objc
result = (a > b) ? x : y;
```

**Not preferred:**

```objc
result = a > b ? x = c > d ? c : d : y;
```

### Multiple conditionals

* When using mutliple conditionals, place the operator on a new line.

```ojc
if (some_long_condition
    && some_other_long_condition
	|| some_completely_different_long_condition) {
	// Do something
}
```

* For complex and multiple conditionals, prefer the following.

**Preferred:**

```objc
BOOL test1 = some_long_condition && some_other_long_condition;
BOOL test2 = some_other_long_condition && some_other_long_condition2;

if (test1 || test2) {
    // Do something
}
```

**Not preferred:**

```
if (some_long_condition && some_other_long_condition || (some_other_long_condition && some_other_long_condition2) ||
    some_completely_different_long_condition) {

}
```


### Yoda Style

To prevent from human mistakes, the following code should be avoided.

**Not preferred:**

```objc
if (myValue == 42) {
    // Do something
}
```

There is a risk the developer may type `myValue = 42`, which will not raise any error.

**Preferred:**

```objc
if (42 == myValue) {
    // Do something
}
```

But if the developer writes `42 = myValue`, it will easily appear as an error.

### Switch

Use braces.

```objc
switch (something.state) {
    case 0: {
        // Do something
    }
        break;
    case 2:
    case 3: {
        // Do something
    }
        break;
    default: {
        // Do something
    }
        break;
}

```

If every cases are covered by the switch, do no implement a default.

**For instance:**

```objc
typedef NS_ENUM(NSInteger, BKTState) {
    BKTStateLoaded,
    BKTStateLoading,
    BKTStateInError
};

switch (state) {
    case BKTStateLoaded: {
        // Do something
    }
        break;
    case BKTStateLoading: {
        // Do something
    }
        break;
    case BKTStateInError: {
        // Do something
    }
        break;
}

```

## Braces

* The opening brace should be on the same line of the method or the control structure (`if`/`else`/`switch`/`while` etc.). The closing brace should be on its own line.
* If a keyword is related and preceded by a closing brace, it should be on the same line.
* There should be a space between a keyboard and the brace / paranthese it is followed / preceded by.

**Preferred:**

```objc
if (!error) {
    return success;
} else {
    return failed;
}
```

**Not preferred**

```objc
if(!error){
    return success;
} else return failed;
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace.

### Class-restricted constants

**Preferred:**

```objc
NSString * const BKTActionNotification = @"Backelite notification";
const CGFloat BKTImageThumbnailHeight = 50.0f;
```

**Not preferred:**

```objc
#define ActionNotification @"Backelite notification"
#define thumbnailHeight 50
```

### Public constants

Constants can be made public by exposing them in the .h file.
`static` should be used to make sure the constant is not duplicated and the instance is unique.

```objc
FOUNDATION_EXPORT static NSString * const BKTActionNotification";
FOUNDATION_EXPORT static const CGFloat BKTImageThumbnailHeight;
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types: `NS_ENUM()`

**For instance:**

```objc
typedef NS_ENUM(NSInteger, BKTState) {
    BKTStateLoaded,
    BKTStateLoading,
    BKTStateInError
};
```

## Bitmasks

When working with bitmasks, the NS_OPTIONS macro MUST be used.

**Example:**

```objc
typedef NS_OPTIONS(NSUInteger, BKTState) {
    BKTStateInError               = 1 << 0,
    BKTStateInErrorWSFailed       = 1 << 1,
    BKTStateInErrorNetwork        = 1 << 2
};
```

## Booleans

Objective-C uses `YES` and `NO`.  Therefore `true` and `false` should only be used for CoreFoundation, C or C++ code. 
Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**Preferred:**

```objc
if (!someObject)
if (isSuper)
if (![someObject boolValue])
```

**Not preferred:**

```objc
if (someObject == nil)
if (isSuper == YES)
if ([someObject boolValue] == NO)
```

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).


## Private properties

Private properties should be declared in class extensions in the implementation file of a class.

**For example:**

```objc
@interface BKTView ()

@property (nonatomic) NSUInteger *bktTag;
@property (nonatomic) BOOL bktViewIsLoaded;

@end
```

## Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

**Preferred:**

```objc
NSError *error;
if (![self doSomethingWithError:&error]) {
    // Handle error
    // doSomethingWithError should return a BOOL in order to consider if the result is ok or not
}
```

**Not preferred:**

```objc
NSError *error;
[self doSomethingWithError:&error];
if (error) {
    // Gérer l'erreur
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).


## Try / catch

**NEVER** use try catch in Objective-C on your own code, except when using a third-party framework that may crash.


## `CGRect` functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Not preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Xcode project

We chose to work with a logical architecture.
A big part of our classes are located in the /Classes folder and represented in Xcode by a logical architecture. 

A regular Backelite project will be structured like the following :

+ YourProjectName
+ - Generic
+ - + Categories
+ - + View
+ - + View Controller
+ - Metier
+ - + FormValidator
+ - + MetierHelper
+ - Model
+ - + BusinessModels
+ - + CityGuide
+ - + Helpers
+ - UI
+ - WS
+ Pods
+ - Lot of pods
