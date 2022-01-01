+++
title = "The hidden UINavigationBar property - large title two line mode"
date = 2020-12-28
description = "-"

[taxonomies]
categories = ["programming"]
tags = ["iOS", "hacks"]
+++

## ToC

1. [Background](#background)
1. [Community solutions](#community-solutions)
1. [Reverse engineering](#reverse-engineering)
1. [Demo project](#demo-project)
1. [The Solution](#the-solution)
1. [Notes](#notes)

## Background

Introduced in iOS 11, the **large title** feature has become a normal UI for iOS users. UIKit framework provides a [simple way](https://developer.apple.com/documentation/uikit/uinavigationbar/2908999-preferslargetitles) to enable the new shiny feature. **But**, there is a missing piece in the API: **We can't make a large title to have 2 lines of text** (_or do we?_)

## Community solutions

If Apple didn't add a way to make two lines large titles, developers made their way to solve the problem, on [this stackoverflow](https://stackoverflow.com/questions/47901318/how-to-set-multi-line-large-title-in-navigation-bar-new-feature-of-ios-11) page, there are multiple solutions, I tried some of them in iOS 15 and couldn't make it work 😩

[khurshedgulov](https://developer.apple.com/forums/profile/khurshedgulov) asked in [apple developer forums](https://developer.apple.com/forums/thread/671982), how to make large titles like **Apple is doing in App Store app**, unfortunately no one from Apple responded.

There are at least 2 apps from Apple, that are using 2 lines for Large Titles: _App Store_, _Watch_

## Reverse engineering

I was very confident that there is a hidden way to enable this feature. **Let's dig ⛏ inside UIKit**

Based on the [stackoverflow](https://stackoverflow.com/questions/47901318/how-to-set-multi-line-large-title-in-navigation-bar-new-feature-of-ios-11) answers, the starting point was a to look into the view structure of `NavigationBar`. I created a subclass to `NavigationBar` and put a breakpoint in the function `didAddSubview`. I inspected every subview that was coming as input parameter to this function. From all subviews, one is named `_UINavigationBarLargeTitleView` (a private Apple class), and it has a private var named `_twoLineMode` with a value of `0` (turned off)!

Two Line Mode - it is there, I just needed a way to turn it on

![private-class](./_UINavigationBarLargeTitleView.png)

_To set a private var, it is done using [Key Value Coding](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/KeyValueCoding/index.html#//apple_ref/doc/uid/10000107i)._ So I tried this

```swift
subview.setValue(1, forKey: "_twoLineMode")
```

I tried to set this value in the `didAddSubview` and `willAddSubview` functions. None worked 😩. The value is reset somewhere else, I verified the value of this var after the view is added to the `UINavigationBar`'s view. After a long inspecting and playing around with different variables of the `_UINavigationBarLargeTitleView` object, I could not make it to work.

I needed to find which object is controlling the `_twoLineMode` var.

`UINavigationBar` is a child view of a `UINavigationController`'s view, so the next move was to dig inside the `NavigationController` class. I read again the documentation and was looking in the header files for `NavigationController` to find a clue.

> A navigation controller builds the contents of the navigation bar dynamically using the navigation item objects (instances of the **UINavigationItem** class) associated with the view controllers on the navigation stack.

`UINavigationItem` 🤔 looks like a clue. To inspect this item, I created a subclass to `UINavigationController` and overrode function `navigationBar(_:shouldPush:)` which is a function for `UINavigationBar`'s delegate. After inspecting this object, I **Found another clue!** Another private var named `__largeTitleTwoLineMode`, again, with a value of `0`.
![__largeTitleTwoLineMode](__largeTitleTwoLineMode.png)

I set this var to `1` or `true` and **HERE IT IS!**. I had a Large Title with 2 lines 🙌!

```swift
item.setValue(1, forKey: "__largeTitleTwoLineMode")
// OR
item.setValue(true, forKey: "__largeTitleTwoLineMode")
```

![TwoLinesLargeTitle](./TwoLinesLargeTitle.png)

## Demo project

I created a demo project named [NavBarLargeTitle](https://github.com/BProg/NavBarLargeTitle). The project will list Swift Language Evolution proposals inside first `UIViewController` and details in the second. It target iOS 15 and uses some iOS 14 features for configuring the `UITableView` like [cell configuration](https://developer.apple.com/documentation/uikit/uitableviewcell/3601058-defaultcontentconfiguration)

## The Solution

1. Create a custom `UINavigationController`
1. Add the protocol `UINavigationBarDelegate` to the class definition
1. Override the function `navigationBar(_:shouldPush:)`
1. Activate two lines mode using hidden variable `item.setValue(true, forKey: "__largeTitleTwoLineMode")`
1. Make `navigationController.navigationBar.prefersLargeTitles = true`

## Notes

### Green 🍀

I tested the solution in iOS 15,14,13 simulators and it works.

### Gray 🌚

This could work with a custom `UINavigationBar` and custom `UINavigationItem`, but I haven't tested it

### Red 🐞

The interactive back animation is not perfect in the demo project, I haven't go further to dig into this problem.

We can't rely on Apple hidden API because it can break with any new release.
