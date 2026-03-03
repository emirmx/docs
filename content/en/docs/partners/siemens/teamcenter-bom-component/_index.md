---
title: "Teamcenter BOM component"
url: /partners/siemens/teamcenter-bom-component/
weight: 20
description: "Describes Teamcenter BOM component, which enables out of the box visualization of large Bill Of Material (BOM) data sets similar to what's available in Teamcenter's Active Workspace."
---

## Introduction
The Teamcenter BOM Widget brings **Teamcenter Bill of Materials (BOM)** capabilities into Mendix applications. Use it to view, edit, configure, and perform property updates on Teamcenter BOM structures directly in Mendix, reducing context switches and simplifying product structure tasks.

### Key Capabilities
* Display Teamcenter BOMs: render complex product structures within Mendix pages.
* Edit BOM properties: update BOM line properties from Mendix.
* Apply Teamcenter configuration: use revision rules and effectivities (date/unit), variants.
* Augment with Mendix data: inject custom **Client Columns** alongside Teamcenter properties.

## Prerequisites
* Teamcenter Connector version 2512.0.0 or higher (the widget depends on its request handler).
* Access to a Teamcenter environment 2512 or above with appropriate credentials/permissions. The widget utilizes new APIs introduced in Teamcenter 2512 and hence cannot be used with older versions of Teamcenter.
* Teamcenter base platform and Structure Management modules installed.
* Mendix 10.24.12 or higher.
* A Mendix application with the TcConnector module added and configured.

## Persistence & Propagation
Edits made through the widget invoke Teamcenter services via `TcConnector`. Changes to BOM line properties are propagated to Teamcenter when the operation is committed and accepted by the server. Ensure users have appropriate permissions, and consider requiring explicit save/commit actions in your UI.

## Notes & Best Practices
* Keep titles and headers concise and technical; avoid marketing language in developer documentation.
* Use unique `WidgetIDs` per page to avoid collisions.