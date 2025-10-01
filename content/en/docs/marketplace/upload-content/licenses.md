---
title: "Open-Source Software Licenses"
url: /appstore/licenses/
weight: 5
description: "Describes the open-source software license options available and their requirements."
---

## Introduction

The following table describes the open-source software license options available and their requirements.

{{% alert color="warning" %}}
Open-source software licenses must abide by a set of compliance rules to ensure the safety of the Mendix ecosystem. Refer to [OSS Compliance for External Developers](/appstore/submit-content/oss-compliance/) for details.
{{% /alert %}}

| | **Notes** | **Commercial use allowed?** | **Component code needs to be in public repo?** | **License text required with copyright info in code and distribution artifact?** | **Can modify?** (Mention modifications to code) | **Can consuming apps use without making their code public?** | **Notice files should be distributed with artifact?** | **Original component source code to be distributed with consuming app?** | **Can sub-license?** |
| --- | --- | --- | --- | --- | --- | --- |  --- | --- | --- |
| [MIT](https://opensource.org/licenses/MIT) | Add a specific *license.txt* file in your artifacts, i.e. in the *.mpk* package. | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="remove-circle-filled" color="red" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="remove-circle-filled" color="red" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} |
| **BSD 2.0, 3.0** | N/A | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="remove-circle-filled" color="red" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} |
| **Apache 1.0** | N/A | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="remove-circle-filled" color="red" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} |
| [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) | Add a specific *license.txt* file in your artifacts, i.e. in the *.mpk* package. | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="remove-circle-filled" color="red" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="remove-circle-filled" color="red" >}} | {{< icon name="checkmark-circle-filled" color="green" >}}  |
| **Creative Commons CC0 1.0 Universal (CC-0)** (Public Domain) | N/A | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="remove-circle-filled" color="red" >}} | {{< icon name="remove-circle-filled" color="red" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} | {{< icon name="remove-circle-filled" color="red" >}} | {{< icon name="remove-circle-filled" color="red" >}} | {{< icon name="checkmark-circle-filled" color="green" >}} |

{{% alert color="info" %}}
The [GNU General Public License (GPL), version 3](https://www.gnu.org/licenses/gpl-3.0.en.html) is not available to use, as everything licensed under GNU GPL is public.    
GNU GPL has a strong copyleft effect.    
Modification has a strong copyleft effect.    
All consuming apps should make their code public.
{{% /alert %}}