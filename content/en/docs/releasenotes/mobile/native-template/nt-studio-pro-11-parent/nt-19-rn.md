---
title: "Native Template 19"
url: /releasenotes/mobile/nt-19-rn/
weight: 7
description: "Native Template 19"
---

## 19.0.0 {#1900}

**Release date: April 20, 2026**

### Improvements

* We upgraded the core stack to React Native `0.83.4` and aligned related React dependencies.
* We upgraded multiple React Native ecosystem dependencies for compatibility and stability, including CLI, navigation, animation, media, and platform modules.
* We migrated from `react-native-vector-icons` to the scoped `@react-native-vector-icons/*` package set.
* We updated `.gitignore` to more precisely exclude `node_modules` directories in specific locations.
* We added `@shopify/flash-list` to support the migration from FlatList to FlashList.
* We upgraded `react-native-tab-view` from `3.5.2` to `4.3.0`.

### Fixes

* We fixed iOS builds crashing when built with Xcode 26.
