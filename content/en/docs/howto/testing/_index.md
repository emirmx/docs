---
title: "Testing"
url: /howto/testing/
weight: 100
description: "Describes the available options for testing your Mendix application."
no_list: false
description_list: true
aliases:
    - /howto/testing/
---

## Introduction

Testing is a vital component when creating a Mendix application. Finding and fixing bugs when your application is in production is more expensive and time-consuming than before a release. Solving issues in the early stages of development ensures a more stable product and better user experience.

## Testing Pyramid

The testing pyramid is a convenient way to categorize tests according to their type, cost, speed, and scope.

{{< figure src="/attachments/howto/testing/testing-pyramid.png" class="no-border" >}}

Tests at the bottom of the pyramid are generally cheaper and faster to run, giving you quicker feedback. As you move up, the scope of the tests increases, but so does their complexity and execution time.

By catching most issues at the lower levels, you reduce the number of expensive, time-consuming UI and E2E tests you need to run, making your testing strategy more efficient. The testing pyramid provides a balanced approach, ensuring comprehensive coverage without over-investing in the most expensive types of tests.

## Mendix Offerings per Test Type

