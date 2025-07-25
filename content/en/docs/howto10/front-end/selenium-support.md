---
title: "Configure Selenium Support"
url: /howto10/front-end/selenium-support/
weight: 16
description: "Describes Selenium best practices, including advice on naming conventions."
aliases:
    - /howto10/integration/selenium-support/
---

## Introduction

Mendix uses CSS classes to identify page content like widgets and pop-up windows. You can use these classes in Selenium to manipulate pages and verify data.

This how-to teaches you how to do the following:

* Use naming conventions
* Use best practices 

## Naming Conventions

Widgets can be given a name in Studio Pro. These names appear in the HTML document as class names prefixed by `mx-name-`. For instance, a grid named `ArtistGrid` will get the `mx-name-ArtistGrid` a CSS class. This is true for all widgets.

More complex widgets, like a grid, can set more specific CSS classes to sub-elements, like columns or the paging bar buttons.

Some widgets, like grids or list views, can show multiple items. Every item has the `mx-name-index-` CSS class followed by an index number, starting at 0.

The easiest way to discover these names is to use a browser's developer tool.

## Best Practices

### Nested Widgets

Every widget has a unique class name, which means that you can use the name on its own as a selector in Selenium. This makes them robust against changes in your page, like moving a button from one container to another. However, not all sub-elements in widgets are unique, like the buttons in a grid's paging bar. To select such an element, use a descendant selector like `.mx-name-artist-grid .mx-name-next`. This first selects the artist grid, then searches for the next page button on that grid.

### Timing Issues

Some actions done by Selenium take time to complete, such as animations or requesting data for a pop-up window. When clicking a search button in a grid, the search bar appears using an animation. This means that after clicking the button, the test needs to wait for the animation to complete before continuing.

For more information, see the WebDriver documentation on [Waiting Strategies](https://www.selenium.dev/documentation/webdriver/waits/).

## Examples

Select a microflow button named `Execute` in a page:

```javascript
$('.mx-name-Execute')

```

Select the fourth row a grid named `ArtistGrid`:

```javascript
$('.mx-name-ArtistGrid .mx-name-index-3')

```

Note that the fourth row in a grid has an index of `3`.

## Read More

* [Test Mendix Applications Using Selenium IDE](/howto10/testing/testing-mendix-applications-using-selenium-ide/)
* [Consume a Complex Web Service](/howto10/integration/consume-a-complex-web-service/)
* [Consume a Simple Web Service](/howto10/integration/consume-a-simple-web-service/)
* [Import Excel Documents](/howto10/integration/importing-excel-documents/)
* [Export XML Documents](/howto10/integration/export-xml-documents/)
* [Expose a Web Service](/howto10/integration/expose-a-web-service/)
* [Import XML Documents](/howto10/integration/importing-xml-documents/)
