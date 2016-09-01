---
layout: post
title: "pod install"
date: 2016-09-01
---
There is an error occur during init the simplenote-ios using pod install.

>TypeError - Unable to convert Ruby value "Reachability" into a CFTypeRef

The reason of the error is that my ruby version is out-of-date, so I upgrade the ruby version to 2.3.0, and the problem disappear.

Reference the [CocoaPods Issues #5200](https://github.com/CocoaPods/CocoaPods/issues/5200)

## Some tips of ruby:

- list the ruby has installed

> rvm list

- change ruby to an exist version

> rvm use 2.3.0
