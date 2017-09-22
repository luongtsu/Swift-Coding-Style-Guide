# Swift-Coding-Style-Guide
The aim of this guide is to help developers in writing clean, concise, beautiful and easy to read Swift codes. The main goals:

- to make it hard to write programmer errors, or at least make them hard to miss
- to increase readability and clarity of intent
- to minimize unnecessary code bloat
- to have a nice look of code


# Table of Contents
- [Styles and Conventions](#styles-and-conventions)
    - [Formatting](#formatting)
        - [Semicolons (`;`)](#semicolons)
        - [Whitespaces](#whitespaces)
        - [Commas (`,`)](#commas)
        - [Colons (`:`)](#colons)
        - [Braces (`{}`)](#braces)
        - [Properties](#properties)
        - [Control Flow Statements](#control-flow-statements)
    - [Naming](#naming)
        - [Capitalization](#capitalization)
        - [Semantics](#semantics)
    - [Dependencies](#dependencies)
        - [Import Statements](#import-statements)
    - [Declaration Order](#declaration-order)
- [Best Practices](#best-practices)
    - [Comments](#comments)
    - [Protection from Dynamism](#protection-from-dynamism)
    - [Access Modifiers](#access-modifiers)
    - [Type Inference](#type-inference)
    - [Collections / SequenceTypes](#collections--sequencetypes)
    - [Protection from Retain Cycles](#protection-from-retain-cycles)


# Styles and Conventions

## Formatting

### Semicolons

#### Trailing semicolons (`;`) are not nessesary any more. This is Swift*

**Preferred**
```swift
self.backgroundColor = UIColor.whiteColor()
self.completion = { 
    // ...
}
```
**Not preferred**
```swift
self.backgroundColor = UIColor.whiteColor();
self.completion = { 
    // ...
};
```

### Whitespaces

#### Use 4 spaces for tabs.
It's preferred to use tab to indent the code instead of using spaces.
Tab setting could be set at Xcode's **Text Editing** settings. As following:
<img width="749" alt="tab setting" src="https://github.com/luongtsu/Swift-Coding-Style-Guide/blob/develop/Images/TabSetting.png" />


#### All source files should end with a single trailing newline (only).
*This prevents no-trailing-newline errors and reduces noise in commit diffs.*

**Preferred**
```swift
class Button {
  // ...
}
// <-- One line here
```
**Not preferred**
```swift
class Button {
  // ...
} // <-- No new line after
```
```swift
class Button {
  // ...
}
// <-- One line here
// <-- Another line here
```


#### All functions should be at least one empty line apart each other.
*Gives breathing room between code blocks.*

**Preferred**
```swift
class BaseViewController: UIViewController {
    // ...
    
    override viewDidLoad() {
        // ...
    }
    
    override viewWillAppear(animated: Bool) {
        // ...
    }
}
```


#### Use single spaces around operator definitions and operator calls.
*Readability*

**Preferred**
```swift
func <| (lhs: Int, rhs: Int) -> Int {
    // ...
}

let value = 1 <| 2
```
**Not Preferred**
```swift
func <|(lhs: Int, rhs: Int) -> Int {
    // ...
}

let value = 1<|2
```


#### Use single spaces around return arrows (`->`) both in functions and in closures.
*Readability*

**Preferred**
```swift
func doSomething(value: Int) -> Int {
    // ...
}
```
**Not Preferred**
```swift
func doSomething(value: Int)->Int {
    // ...
}
```


### Commas

#### Commas (`,`) should have no whitespace before it, and should have either one space or one newline after.
*Keeps comma-separated items visually separate.*

**Preferred**
```swift
let array = [1, 2, 3]
```
```swift
self.presentViewController(
    controller,
    animated: true,
    completion: nil
)
```
**Not Preferred**
```swift
let array = [1,2,3]
let array = [1 ,2 ,3]
let array = [1 , 2 , 3]
```
```swift
self.presentViewController(
    controller ,
    animated: true,completion: nil
)
```


### Colons

#### Colons (`:`) used to indicate type should have one space after it and should have no whitespace before it.
*The colon describes the object to its left, not the right.*

**Preferred**
```swift
func createItem(item: Item)
```
```swift
var item: Item? = nil
```
**Not Preferred**
```swift
func createItem(item:Item)
func createItem(item :Item)
func createItem(item : Item)
```
```swift
var item:Item? = nil
var item :Item? = nil
var item : Item? = nil
```


#### Colons (`:`) for `case` statements should have no whitespace before it, and should have either one space or one newline after it.
*Same as he previous rule, the colon describes the object to its left, not the right.*

**Preferred**
```swift
switch result {

case .Success:
    self.completion()
    
case .Failure:
    self.failure()
}
```
**Not Preferred**
```swift
switch result {

case .Success :
    self.completion()
    
case .Failure:self.reportError()
}
```


### Braces

#### Open braces (`{`) should be one space following the previous non-whitespace character.
*Separates the brace from the declaration.*

**Preferred**
```swift
class Icon {
    // ...
}
```
```swift
let block = { () -> Void in
    // ...
}
```
**Not Preferred**
```swift
class Icon{
    // ...
}
```
```swift
let block ={ () -> Void in
    // ...
}
```


#### Open braces (`{`) for type declarations, functions, and closures should be followed by one empty line. Single-statement closures can be written in one line.
*Gives breathing room when scanning for code.*

**Preferred**
```swift
class Icon {

    let image: UIImage
    var completion: (() -> Void)

    init(image: UIImage) {
    
        self.image = image
        self.completion = { [weak self] in self?.didComplete() }
    }
    
    func doSomething() {
    
        self.doSomethingElse()
    }
}
```
**Not Preferred**
```swift
class Icon {
    let image: UIImage

    init(image: UIImage) {
        self.image = image
        self.completion = { [weak self] in print("done"); self?.didComplete() }
    }
    
    func doSomething() { self.doSomethingElse() }
}
```


#### Empty declarations should be written in empty braces (`{}`), otherwise a comment should indicate the reason for the empty implementation.
*Makes it clear that the declaration was meant to be empty and not just a missing `TODO`.*

**Preferred**
```swift
extension Icon: Equatable {}
```
```swift
var didTap: () -> Void = {}

override func drawRect(rect: CGRect) {}

@objc dynamic func controllerDidChangeContent(controller: NSFetchedResultsController) {

    // do nothing; delegate method required to enable tracking mode
}
```
**Not Preferred**
```swift
extension Icon: Equatable {
}
```
```swift
var didTap: () -> Void = { }

override func drawRect(rect: CGRect) {
}

@objc dynamic func controllerDidChangeContent(controller: NSFetchedResultsController) {
    
}
```

#### Close braces (`}`) should not have empty lines before it. For single line expressions enclosed in braces, there should be one space between the last statement and the closing brace.
*Provides breathing room between declarations while keeping code compact.*

**Preferred**
```swift
class Button {

    var didTap: (sender: Button) -> Void = { _ in }

    func tap() {
    
        self.didTap()
    }
}
```
**Not Preferred**
```swift
class Button {

    var didTap: (sender: Button) -> Void = {_ in}

    func tap() {
    
        self.didTap()
        
    }
    
}
```


#### Close braces (`}`) unless on the same line as its corresponding open brace (`{`), should be left-aligned with the statement that declared the open brace.
*Close braces left-aligned with their opening statements visually express their scopes pretty well. This rule is the basis for the succeeding formatting guidelines below.*

**Preferred**
```swift
lazy var largeImage: UIImage = { () -> UIImage in

    let image = // ...
    return image
}()
```
**Not Preferred**
```swift
lazy var largeImage: UIImage = { () -> UIImage in

    let image = // ...
    return image
    }()
```


### Properties

#### The `get` and `set` statement and their close braces (`}`) should all be left-aligned. If the statement in the braces can be expressed in a single line, the `get` and `set` declaration can be inlined.
*Combined with the [rules on braces](#braces), this formatting provides very good consistency and scannability.*

**Preferred**
```swift
struct Rectangle {

    // ...
    var right: Float {
    
        get {
        
            return self.x + self.width
        }
        set {
        
            self.x = newValue - self.width
        }
    }
}
```
```swift
struct Rectangle {

    // ...
    var right: Float {
    
        get { return self.x + self.width }
        set { self.x = newValue - self.width }
    }
}
```
**Not Preferred**
```swift
struct Rectangle {

    // ...
    var right: Float {
    
        get
        {
            return self.x + self.width
        }
        set
        {
            self.x = newValue - self.width
        }
    }
}
```
```swift
struct Rectangle {

    // ...
    var right: Float {
    
        get { return self.x + self.width }
        set { self.x = newValue - self.width
            print(self) 
        }
    }
}
```


#### Read-only computed properties should ommit the `get` clause.
*The `return` statement provides enough clarity that lets us use the more compact form.*

**Preferred**
```swift
struct Rectangle {

    // ...
    var right: Float {
    
        return self.x + self.width
    }
}
```
**Not Preferred**
```swift
struct Rectangle {

    // ...
    var right: Float {
    
        get {
        
            return self.x + self.width
        }
    }
}
```


### Control Flow Statements

#### `if`, `else`, `switch`, `do`, `catch`, `repeat`, `guard`, `for`, `while`, and `defer` statements should be left-aligned with their respective close braces (`}`).
*Combined with the [rules on braces](#braces), this formatting provides very good consistency and scannability. Close braces left-aligned with their respective control flow statements visually express their scopes pretty well.*

**Preferred**
```swift
if array.isEmpty {
    // ...
}
else {
    // ...
}
```
*or*
```swift
if array.isEmpty {
    // ...
} else {
    // ...
}
```
*Personally, I like the second way*
**Not Preferred**
```swift
if array.isEmpty
{
    // ...
}
else
{
    // ...
}
```


#### `case` statements should be left-aligned with the `switch` statement. Single-line `case` statements can be inlined and written compact. Multi-line `case` statements should be indented below `case:` and separated with one empty line.
*Reliance on Xcode's auto-indentation. For multi-line statements, separating `case`s with empty lines enhance visual separation.*

**Preferred**
```swift
switch result {

case .Success:
    self.doSomething()
    self.doSomethingElse()
    
case .Failure:
    self.doSomething()
    self.doSomethingElse()
}
```
```swift
switch result {

case .Success: self.doSomething()
case .Failure: self.doSomethingElse()
}
```
**Not Preferred**
```swift
switch result {

case .Success: self.doSomething()
               self.doSomethingElse()
case .Failure: self.doSomething()
               self.doSomethingElse()
}
```
```swift
switch result {

    case .Success: self.doSomething()
    case .Failure: self.doSomethingElse()
}
``` 


#### Conditions for `if`, `switch`, `for`, and `while` statements should not be enclosed in parentheses (`()`).
*Do coding Swift way*

**Preferred**
```swift
if array.isEmpty {
    // ...
}
```
**Not Preferred**
```swift
if (array.isEmpty) {
    // ...
}
``` 

#### Try to avoid nesting statements by `return`ing early when possible.
*The more nested scopes to keep track of, the heavier the burden of scanning code.*

**Preferred**
```swift
guard let strongSelf = self else {

    return
}
// do many things with strongSelf
```
**Not Preferred**
```swift
if let strongSelf = self {

    // do many things with strongSelf
}
``` 



## Naming

Naming rules are mostly based on Apple's naming conventions, since we'll end up consuming their API anyway.

### Capitalization

#### Type names (`class`, `struct`, `enum`, `protocol`) should be in *UpperCamelCase*. 
*Adopt Apple's naming rules for uniformity.*

**Preferred**
```swift
class ImageButton {

    enum ButtonState {
        // ...
    }
}
```
**Not Preferred**
```swift
class image_button {

    enum buttonState {
        // ...
    }
}
```


#### `enum` values and `OptionSetType` values should be in *UpperCamelCase*. 
*Adopt Apple's naming rules for uniformity.*

**Preferred**
```swift
enum ErrorCode {
    
    case Unknown
      case NetworkNotFound
      case InvalidParameters
}

struct CacheOptions : OptionSetType {

    static let None = CacheOptions(rawValue: 0)
    static let MemoryOnly = CacheOptions(rawValue: 1)
    static let DiskOnly = CacheOptions(rawValue: 2)
    static let All: CacheOptions = [.MemoryOnly, .DiskOnly]
    // ...
}
```
**Not Preferred**
```swift
enum ErrorCode {
    
    case unknown
      case network_not_found
      case invalidParameters
}

struct CacheOptions : OptionSetType {

    static let none = CacheOptions(rawValue: 0)
    static let memory_only = CacheOptions(rawValue: 1)
    static let diskOnly = CacheOptions(rawValue: 2)
    static let all: CacheOptions = [.memory_only, .diskOnly]
    // ...
}
```


#### Variables and functions should be in *lowerCamelCase*, including statics and constants. An exception is acronyms, which should be *UPPERCASE*.
*Adopt Apple's naming rules for uniformity. As for acronyms, the readability makes keeping them upper-case worth it.*

**Preferred**
```swift
var webView: UIWebView?
var URLString: String?

func didTapReloadButton() {
    // ..
}
```
**Not Preferred**
```swift
var web_view: UIWebView?
var urlString: String?

func DidTapReloadButton() {
    // ..
}
```


### Semantics

#### Avoid single-character names for types, variables, and functions. The only place they are allowed is as indexes in iterators.
*There is always a better name than single-character names. Even with `i`, it is still more readable to use `index` instead.*

**Preferred**
```swift
for (i, value) in array.enumerate() {
    // ... "i" is well known
}
```
**Not Preferred**
```swift
for (i, v) in array.enumerate() {
    // ... what's "v"?
}
```


#### Avoid abbreviations as much as possible. (although [Acceptable Abbreviations and Acronyms](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/APIAbbreviations.html#//apple_ref/doc/uid/20001285-BCIHCGAE) are allowed such as `min`/`max`). 
*Clarity is prioritized over slight brevity.*

**Preferred**
```swift
let errorCode = error.code
```
**Not Preferred**
```swift
let err = error.code
```

#### Choose a name that communicates as much information about what it is and *what it's for*.
*Clarity is prioritized over slight brevity. Also, the more specific the name, the less likely they are to collide with other symbols.*

**Preferred**
```swift
class Article {

    var title: String
}
```
```swift
class NewsArticle {

    var headlineTitle: String
}
```
**Not Preferred**
```swift
class Article {

    var text: String
    // is this the title or the content text?
}
```


#### When pertaining to URLs, distinguish strings from actual `NSURL`s by appending the suffix `~String`.
*Saves a few seconds checking header declarations for the correct type.*

**Preferred**
```swift
var requestURL: NSURL
var sourceURLString: String

func loadURL(URL: NSURL) {
    // ...
}

func loadURLString(URLString: String) {
    // ...
}
```
**Not Preferred**
```swift
var requestURL: NSURL
var sourceURL: String

func loadURL(URL: NSURL) {
    // ...
}

func loadURL(URL: String) {
    // ...
}
```


#### Do not pertain to constructs (`class`, `struct`, `enum`, `protocol`, etc.) in their names.
*The extra suffix is redundant. It should be noted though that Objective-C protocols with the same name as an existing Objective-C class are bridged to Swift with a `~Protocol` suffix (e.g. `NSObject` and `NSObjectProtocol`). But they are irrelevant to this guideline as they are automatically generated by the Swift compiler.*

**Preferred**
```swift
class User {
    // ...
}

enum Result {
    // ...
}

protocol Queryable {
    // ...
}
```
**Not Preferred**
```swift
class UserClass {
    // ...
}

enum ResultEnum {
    // ...
}

protocol QueryableProtocol {
    // ...
}
```


## Dependencies

### Import Statements

#### `import` statements for OS frameworks and external frameworks should be separated and alphabetized.
*Reduce merge conflicts when dependencies change between branches.*

**Preferred**
```swift
import Foundation
import UIKit

import Alamofire
import Cartography
import SwiftyJSON
```
**Not Preferred**
```swift
import Foundation
import Alamofire
import SwiftyJSON
import UIKit
import Cartography
```


## Declaration 

#### All properties and methods should be grouped into the superclass/protocol they implement and should be tagged with `// MARK: <superclass/protocol name>`. The rest should be marked as either `// MARK: Public`, `// MARK: Internal`, or `// MARK: Private`.
*Makes it easy to locate where in the source code certain properties and functions are declared.*

**Preferred**
```swift
// MARK: - BaseViewController

class BaseViewController: UIViewController, UIScrollViewDelegate {


    // MARK: Internal
    
    weak var scrollView: UIScrollView?
    
    
    // MARK: UIViewController
    
    override func viewDidLoad() {
        // ...
    }
    
    override func viewWillAppear(animated: Bool) {
        // ...
    }
    
    
    // MARK: UIScrollViewDelegate
    
    @objc dynamic func scrollViewDidScroll(scrollView: UIScrollView) {
        // ...
    }
    
    
    // MARK: Private
    
    private var lastOffset = CGPoint.zero
}
```


#### All `// MARK:` tags should have two empty lines above and  one empty line below.
*Aesthetic. Gives breathing room between type declarations and function groups.*

**Preferred**
```swift
import UIKit


// MARK: - BaseViewController

class BaseViewController: UIViewController {


    // MARK: Internal
    
    weak var scrollView: UIScrollView?
    
    
    // MARK: UIViewController
    
    override func viewDidLoad() {
        // ...
    }
    
    override func viewWillAppear(animated: Bool) {
        // ...
    }
    
    
    // MARK: Private
    
    private var lastOffset = CGPoint.zero
}
```
**Not Preferred**
```swift
import UIKit
// MARK: - BaseViewController
class BaseViewController: UIViewController {

    // MARK: Internal
    weak var scrollView: UIScrollView?
    
    // MARK: UIViewController
    override func viewDidLoad() {
        // ...
    }
    
    override func viewWillAppear(animated: Bool) {
        // ...
    }
    
    // MARK: Private
    private var lastOffset = CGPoint.zero
}
```


#### The groupings for `// MARK:` tags should be ordered as follows:
*Makes it easy to locate where in the source code certain implementations are declared. `public` and `internal` declarations are more likely to be referred to by API consumers, so are declared at the top.*
- `// MARK: Public`
- `// MARK: Internal`
- Class Inheritance (parent-most to child-most)
    - `// MARK: NSObject`
    - `// MARK: UIResponder`
    - `// MARK: UIViewController`
- Protocol Inheritance (parent-most to child-most)
    - `// MARK: UITableViewDataSource`
    - `// MARK: UIScrollViewDelegate `
    - `// MARK: UITableViewDelegate`
- `// MARK: Private`


#### Under each grouping above, declarations should be ordered as follows:
- `@` properties (`@NSManaged`, `@IBOutlet`, `@IBInspectable`, `@objc`, `@nonobjc`, etc.)
- `lazy var` properties
- computed `var` properties
- other `var` properties
- `let` properties
- `@` functions (`@NSManaged`, `@IBAction`, `@objc`, `@nonobjc`, etc.)
- other functions

*`@` properties and functions are more likely to be referred to (such as when checking KVC keys or `Selector` strings, or when cross-referencing with Interface Builder) so are declared higher.*


# Best Practices

In general, **all Xcode warnings should not be ignored**. These include things like using `let` instead of `var` when possible, using `_` in place of unused variables, etc.


## Comments

#### Comments should be answering some form of "why?" question. Anything else should be explainable by the code itself, or not written at all.
*The best comment is the ones you don't need. If you have to write one be sure to explain the rationale behind the code, not just to simply state the obvious.*

**Preferred**
```swift
let leftMargin: CGFloat = 20
view.frame.x = leftMargin
```
```swift
@objc dynamic func tableView(tableView: UITableView,
 heightForHeaderInSection section: Int) -> CGFloat {

    return 0.01 // tableView ignores 0
}
```
**Not Preferred**
```swift
view.frame.x = 20 // left margin
```
```swift
@objc dynamic func tableView(tableView: UITableView,
 heightForHeaderInSection section: Int) -> CGFloat {

    return 0.01 // return small number
}
```


#### All temporary, unlocalized strings should be marked with `// TODO: localize`
*Features are usually debugged and tested in the native language and translated strings are usually tested separately. This guarantees that all unlocalized texts are accounted for and easy to find later on.*

**Preferred**
```swift
self.titleLabel.text = "Date Today:" // TODO: localize
```
**Not Preferred**
```swift
self.titleLabel.text = "Date Today:"
```



## Protection from Dynamism

#### All Objective-C `protocol` implementations, whether properties or methods, should be prefixed with `@objc dynamic`
*Prevents horrible compiler optimization bugs.*

**Preferred**
```swift
@objc dynamic func scrollViewDidScroll(scrollView: UIScrollView) {
    // ...
}
```
**Not Preferred**
```swift
func scrollViewDidScroll(scrollView: UIScrollView) {
    // ...
}
```


#### All `IBAction`s and `IBOutlet`s should be declared `dynamic`
*Xcode automatically generates this for us when we drag&drop from storyboard to viewcontroller*

**Preferred**
```swift
@IBOutlet private dynamic weak var closeButton: UIButton?

@IBAction private dynamic func closeButtonTouchUpInside(sender: UIButton) {
    // ...
}
``` 


#### All `@IBOutlet`s should be declared `weak`. They should also be wrapped as `Optional`, not `ImplicitlyUnwrappedOptional`.
*This guarantees safety even if subclasses opt to not create the view for the `@IBOutlet`. This also protects against crashes caused by properties being accessed before `viewDidLoad(_:)`.*

**Preferred**
```swift
@IBOutlet dynamic weak var profileIcon: UIImageView?
```
**Not Preferred**
```swift
@IBOutlet var profileIcon: UIImageView!
```



## Access Modifiers

#### Design declarations as `private` by default and only expose as `internal` or `public` as the needs arise.
*This helps prevent pollution of XCode's auto-completion. In theory this should also help the compiler make better optimizations and build faster.*


#### For library modules: all declarations should explicitly specify either `public`, `internal`, or `private`.
*Makes the intent clear for API consumers.*

**Preferred**
```swift
private let defaultTimeout: NSTimeInterval = 30

internal class NetworkRequest {
    // ...
}
```
**Not Preferred**
```swift
let defaultTimeout: NSTimeInterval = 30

class NetworkRequest {
    // ...
}
```


#### For application modules: `public` access is prohibited unless required by a protocol. The `internal` keyword may or may not be written, but the `private` keyword is required.
*A `public` declaration in an app bundle does not make sense. In effect, declarations are assumed to be either `internal` or `private`, in which case it is sufficient to just require `private` explicitly.*

**Preferred**
```swift
private let someGlobal = "someValue"

class AppDelegate {
    // ...
    private var isForeground = false
}
```
**Not Preferred**
```swift
public let someGlobal = "someValue"

public class AppDelegate {
    // ...
    var isForeground = false
}
```


#### Access modifiers should be written before all other non-`@` modifiers.
Combined with the [rules on declaration order](#declaration-order), this improves readability when scanning code vertically*

**Preferred**
```swift
@objc internal class User: NSManagedObject {
    // ...
    @NSManaged internal dynamic var identifier: Int
    // ...
    @NSManaged private dynamic var internalCache: NSData?
}
```
**Not Preferred**
```swift
internal @objc class User: NSManagedObject {
    // ...
    @NSManaged dynamic internal var identifier: Int
    // ...
    private @NSManaged dynamic var internalCache: NSData?
}
```



## Type Inference

#### Unless required, a variable/property declaration's type should be inferred from either the left or right side of the statement, but not both.
*Prevent redundancy. This also reduces ambiguity when binding to generic types.*

**Preferred**
```swift
var backgroundColor = UIColor.whiteColor()
var iconView = UIImageView(image)
```
```swift
var lineBreakMode = NSLineBreakMode.ByWordWrapping
// or
var lineBreakMode: NSLineBreakMode = .ByWordWrapping
```
**Not Preferred**
```swift
var backgroundColor: UIColor = UIColor.whiteColor()
var iconView: UIImageView = UIImageView(image)
```
```swift
var lineBreakMode: NSLineBreakMode = NSLineBreakMode.ByWordWrapping
```


#### When literal types are involved (`StringLiteralConvertible`, `NilLiteralConvertible`, etc), it is encouraged to specify the type explicitly and is preferrable over casting with `as` directly.
*Prevent redundancy. This also reduces ambiguity when binding to generic types.*

**Preferred**
```swift
var radius: CGFloat = 0
var length = CGFloat(0)
```
**Not Preferred**
```swift
var radius: CGFloat = CGFloat(0)
var length = 0 as CGFloat // prefer initializer to casts
```

## Collections / SequenceTypes

#### `.count` should only be used when the count value itself is needed
**Preferred**
```swift
let badgeNumber = unreadItems.count
```
*Checking if empty or not:*
```swift
if sequence.isEmpty {
// ...
```
**Not Preferred**
```swift
if sequence.count <= 0 {
// ...
```

#### Getting the first or last item:
**Preferred**
```swift
let first = sequence.first
let last = sequence.last
```
**Not Preferred**
```swift
let first = sequence[0]
let last = sequence[sequence.count - 1]
```

#### Removing the first or last item:
**Preferred**
```swift
sequence.removeFirst()
sequence.removeLast()
```
**Not Preferred**
```swift
sequence.removeAtIndex(0)
sequence.removeAtIndex(sequence.count - 1)
```

#### Iterating all indexes:
**Preferred**
```swift
for i in sequence.indices {
    // ...
}
```
**Not Preferred**
```swift
for i in 0 ..< sequence.count {
    // ...
}
```

#### Getting the first or last index:
**Preferred**
```swift
let first = sequence.indices.first
let last = sequence.indices.last
```
**Not Preferred**
```swift
let first = 0
let last = sequence.count - 1
```

#### Iterating all indexes except the last `n` indexes:
**Preferred**
```swift
for i in sequence.indices.dropLast(n) {
    // ...
}
```
**Not Preferred**
```swift
for i in 0 ..< (sequence.count - n) {
    // ...
}
```

#### Iterating all indexes except the first `n` indexes:
**Preferred**
```swift
for i in sequence.indices.dropFirst(n) {
    // ...
}
```
**Not Preferred**
```swift
for i in n ..< sequence.count {
    // ...
}
```

*Clarity of intent, which in turn reduces programming mistakes (esp. [off-by-one calculation errors](https://en.wikipedia.org/wiki/Off-by-one_error)).*


## Protection from Retain Cycles 

In particular, this will cover the ever-debatable usage/non-usage of `self`.

#### For all non-`@noescape` and non-animation closures, accessing `self` within the closure requires a `[weak self]` declaration.
*Combined with the `self`-requirement rule above, retain cycle candidates are very easy to spot. Just look for closures that access `self` and check for the missing `[weak self]`. *

**Preferred**
```swift
self.request.downloadImage(
    url,
    completion: { [weak self] image in

        self?.didDownloadImage(image)
    }
)
```
**Not Preferred**
```swift
self.request.downloadImage(
    url,
    completion: { image in

        self.didDownloadImage(image) // hello retain cycle
    }
)
```


#### Never use `unowned` to capture references in closures.
*While `unowned` is more convenient (you don't need to work with an `Optional`) than `weak`, it is also more prone to crashes. Nobody likes zombies.*

**Preferred**
```swift
self.request.downloadImage(
    url,
    completion: { [weak self] image in

        self?.didDownloadImage(image)
    }
)
```
**Not Preferred**
```swift
self.request.downloadImage(
    url,
    completion: { [unowned self] image in

        self.didDownloadImage(image)
    }
)
```


#### If the validity of the weak `self` in the closure is needed, bind using the variable `` `self` `` to shadow the original.
*Write the nice looking code*

**Preferred**
```swift
self.request.downloadImage(
    url,
    completion: { [weak self] image in

        guard let `self` = self else { 
        
            return
        }
        self.didDownloadImage(image)
        self.reloadData()
        self.doSomethingElse()
    }
)
```
**Not Preferred**
```swift
self.request.downloadImage(
    url,
    completion: { [weak self] image in

        guard let strongSelf = self else { 
        
            return
        }
        strongSelf.didDownloadImage(image)
        strongSelf.reloadData()
        strongSelf.doSomethingElse()
    }
)
```

---

[Back to top](#table-of-contents)
