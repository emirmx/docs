---
title: "Rules"
url: /refguide10/rules/
weight: 30
#If moving or renaming this doc file, implement a temporary redirect and let the respective team know they should update the URL in the product. See Mapping to Products for more details.
---

## Introduction

A rule is a special kind of microflow. Its result should be an enumeration or a Boolean and it can be used in a [decision](/refguide10/decision/) to make a decision based on that result. The idea is that complicated decisions can be consolidated in rules and reused in various places.

## Differences from Microflows

Rules are very similar to microflows; see the documentation on [Microflows](/refguide10/microflows/) for more information on how to build a rule. There are only a few differences between rules and microflows:

* A rule can only be used in a decision
* The return type has to be Boolean or enumeration
* A rule cannot change data in the database; the actions to create, delete, change and rollback objects are not available in rules
* A rule cannot interact with the client; the actions to show or close forms, show messages, send validation feedback and download files are not available in rules
* A rule cannot call web services, generate documents or import XML

These differences mean that rules perform better than microflows if you don't need any of the additional features which microflows provide.

## Keyboard Support

For an overview of the shortcut keys that are supported in the rule editor, see the [Microflow, Nanoflow, and Rule Editor Shortcut Keys](/refguide10/keyboard-shortcuts/#logic-editor-keyboard-support) section in *Keyboard Shortcuts*.

## Read More

* [Microflows](/refguide10/microflows/)
* [Decision](/refguide10/decision/)
