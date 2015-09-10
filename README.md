# Xcode: Warnings and Errors

<img src="https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_warning_icon.png" width=200>
<img src="https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_error_icon.png" width=200>

## Objectives

1. Learn the distinction between warning and errors and how to navigate to them with the issue navigator.
2. Recognize common issues and how to fix them:
  * Unused variable
  * Incompatible integer to pointer conversion
  * The return statement ends the method
  * Unmatched curly braces `{` `}`
  * Local variables and method calls written outside of a method body


## Introduction

When using Xcode, you're going to encounter warnings and errors. This is a normal part of the development process, so don't be alarmed when you see these symbols: they're there to help you!

Warnings and errors are generated by the pre-compiler (though often referred to as just "the compiler"), which is a tool built into Xcode that continuously scans through the code *as it's edited* in order to identify problems.

<img src="https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_warning_icon.png" width=24> A **warning** is a minor issue that something unexpected might be caused by the code, but won't necessarily cause a crash at run time. Warnings will not impede the scheme being built.

<img src="https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_error_icon.png" width=24> An **error** is a major issue that will almost certainly cause a crash at run time, so the error impedes the scheme from being built.

Like all of the tools bundled together in Xcode, the pre-compiler is imperfect. It isn't able to identify every possible problem that might come about, and the messages that accompany these warnings and errors aren't always direct; this often leads Xcode to generate cryptic messages and to suggest incorrect fixes.

Feeling confused is normal, but **don't make the assumption that Xcode knows more than you do.** It's an entire sub-skill within iOS development to be able to interpret the underlying cause of the warning or error message and be able to apply the appropriate change. With experience, you'll come to see Xcode like a canine companion that, while intelligent in its own way, is more capable of "sniffing out" (detecting) problems than it is capable of knowing what do about them like a [mountain guru](http://tvtropes.org/pmwiki/pmwiki.php/Main/HermitGuru). 

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_forgot_how_to_wolf.png)

That being said, if your watchdog starts barking—investigate. Meaning, don't simply ignore warnings and errors that Xcode presents to you. They're indicators that you may have done something undesirable, signs that your program might behave in a way that you don't want it to. So look into them. Endeavor to keep your code clean from issues. (*Eventually you'll begin working with other programmers in groups who shouldn't be burdened with having to sift through the warnings meant for you when working on their own features in the same project.*)

The rest of this lesson is an interactive walkthrough to show you some basic warnings and errors that you might see in Xcode and to gain comfort in addressing them.

## Instructions

Fork and clone this lab. 

Open the `objc-xcode-warnings-and-errors.xcodeproj` file and navigate to the `FISAppDelegate.m` file.

### Unused variable

Upon opening the project, you should see a warning pop up pointing out the `NSString` variable called `unused`. Clicking on the warning symbol will highlight the line in yellow and display a warning message. 

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_unused_variable.png)

This is an "Unused variable" warning that pops up whenever a variable has been created but not utilized. It's simply Xcode pointing out that there's a loose end to the logical flow.

You'll notice that the warning symbol also appears in the tool bar, and that the warning message also displays in the Issue Navigator in the Navigator area. These tools can let you quickly jump to the different places in the code where an issue has been identified.

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_unused_variable_full.png)

Go ahead and run the program with `⌘` `R`. Notice that the build succeeds despite the warning.

Now, uncomment the following line of code containing `NSLog(@"%@", unused);`. In addition to printing the `unused` string to the console, this now ties down the loose end of not have used that variable which makes the warning disappear. Run the program again and you should see the string print to the console.

### Incompatible integer to pointer conversion

If you happen to declare a primitive using `*`, the variable you create will get assigned as a pointer reference (which is different from assigning it to the value).

Uncomment the line `NSInteger *i = 0;`.

This is actually permitted, so the compiler doesn't generate a warning until the variable is used in a way not befitting to it. Uncomment the line containing `NSLog(@"i: %li", i);`. You should see a "format specifier" warning that's pretty subtle: it's saying that `'long'` cannot match with `'long *'`. Run the program, you'll see that the build succeeds and the line prints `i: 0`.

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_integer_format.png)

Now change the initial assignment from `0` to anything non-zero, such as `12`. You'll see an "invalid integer to pointer conversion" warning now. Run the program, you'll see that `i: 12` prints. While this has the appearance of working, it's not performing in the way you probably expect.

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_integer_conversion.png)

Uncomment the next two lines that contain `NSInteger x = i + 1;` and `NSLog(@"x: %li", x);`. Xcode will underline the operation:

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_integer_operation.png)

Click on the warning and you'll see a similar "Incompatible integer to pointer conversion" message, but also Xcode offers a fix-it option, translating the `i + 1` into `*(i + 1)`. 

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_integer_fixit.png)

Surely, Xcode knows what it's doing... right? Let's see if does: accept the change and run the program. When you do, you'll see this:

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_integer_crash.png)

That doesn't look good. What's that green line reading `Thread 1: EXC_BAD_ACCESS`? That's a crash caused by Xcode's "fix it".

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_clumsy_canine.jpg)  
—*Thanks for the help, Xcode.*

The actual solution to this problem is removing the `*` from the declaration of `i`. Do that now so the line reads `NSInteger i = 12;`. Replace the assignment of `x` to the original formula `x + 1`, then run the program again. You should see both lines print in the console:

```
i: 12
x: 13
```
Whenever you see an "Incompatible integer to pointer conversion" warning pop up, start checking your primitive variable definitions for extraneous `*`s.

#### The `return` statement ends the method

An executed `return` statement will end the method implementation, meaning that anything written after it will not get run. Take note that the `NSLog()` following the `return YES;` statement does not get executed.

#### A missing closing brace `}` confuses Xcode

Because the curly braces `{` `}` are used to define scopes (we'll explain this concept in more detail a little later), an imbalance between opening and closing curly braces will confuse the pre-compiler's scan of the file.

Delete the closing curly brace below the `return YES;` statement. You'll see a group of three errors pop up:

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_missing_closing_brace.png)

One of those errors seems correct, "Expected '}'". So why the other two? Because the method isn't closed, the `@end` is now inside the the method implementation meaning it isn't part of the larger file, so Xcode gets confused even though it's a simple fix. Add the closing curly brace back in, then insert an unmatched opening curly brace `{` above the `return YES;` statement. This produces the same three errors:

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_extra_opening_brace.png)

Whenever you see those three errors together, start checking your curly braces. This is also why style guides discuss guidelines for keeping track of curly braces. 

#### Local variables and method calls do not belong outside of method bodies

Scroll down to the bottom of the file. Uncomment the three lines of code containing the `NSString *message`, `NSInteger j`, and `BOOL itIsKnownKhaleesi` variables. Notice that no warnings or errors pop up even though this is highly problematic. Just to be sure, build the scheme without running it by pressing `⌘` `B`. 

Still no errors?

Uncomment the remaining lines of code at the end of the file. You'll get assaulted with an army of warnings and errors:

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_so_many_issues.png)
—*AHH!!! What do they all mean!?!*

But again, this is a simple fix: the variables and method calls just don't belong outside of a method body. This is why the code-alongs have you entering code in the `application:didFinishLaunchingWithOptions:` method. Move all of this code inside that method (within the curly braces) and the warnings should disappear. If they hang around, try pressing `⇧` `⌘` `K` (shift+command+k) to "clean" the issues.

Now you're on your way to becoming the mystical guru that Xcode is not:

![](https://curriculum-content.s3.amazonaws.com/ios/ios-objc-fundamentals-unit/xcode_mountain_guru.jpg)




