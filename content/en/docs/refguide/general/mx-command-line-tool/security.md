---
title: "Security Overview Commands"
url: /refguide/mx-command-line-tool/security/
weight: 50
description: "Describes the commands related to the Security Overview"
---

## Introduction

The commands in this group are related to the [Security Overview](/refguide/security-overview/).

## mx export-security-overview Command {#export-security-overview}

This command can be used to export the data in the Security Overview to either a JSON or xlsx file.

### Usage

Use the following command pattern: `mx export-security-overview [OPTIONS] [MPR-FILE]`

These are the `OPTIONS`:

| Option                    | Value             | Result 
|---------------------------|-------------------|----------
| `-t, --export-format`     | `json` or `xlsx`  | The format to export to
| `-e, --exclude-appstore`  | *-*               | When set, exclude marketplace modules
| `-o, --output-file`       | file path         | The path to the output file

### Examples

This is an example:

`mx export-security-overview -t json -e -o C:\MyApp\export.json C:\MyApp\MyApp.mpr`

### Return Codes

These are the return codes:

| Return Code   | Description                   |
| --------------| ----------------------------- |
| 0             | Success                       |
| 200           | An internal error occurred    |
| 400           | The MPR could not be loaded   |