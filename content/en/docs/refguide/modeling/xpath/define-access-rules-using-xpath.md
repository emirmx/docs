---
title: "Defining Access Rules Using XPath"
linktitle: "Define Access Rules Using XPath"
url: /refguide/define-access-rules-using-xpath/
weight: 4
description: "Describes how to define access rules for an entity using an XPath constraint."
aliases:
    - /howto/logic-business-rules/define-access-rules-using-xpath/
---

## Introduction

The access rules of an entity define what a user is allowed to do with the objects of the entity. Users can be allowed to create and/or delete objects and to view and/or edit member values. A member is an attribute or an association of an entity. Furthermore, the set of objects available for viewing, editing, and removing can be limited by means of an XPath constraint (for details, see [XPath Constraints](/refguide/xpath-constraints/) in the *Studio Pro Guide*). For more information on access rules, see [Access Rules](/refguide/access-rules/) in the *Studio Pro Guide*.

In this how-to, you will prepare a data structure (including security), a GUI, and some example data for customers, orders, and a financial administrator account. After this preparation, you will define the access rules for the **Order** entity using XPath on the payment status. The XPath will constrain the order so that it can only be seen by a financial administrator when the payment status of the order is set to **paid**.

This how-to teaches you how to do the following:

* Define access rules for an entity using XPath

## Preparing the Data Structure, GUI, and Example Data

The access rules used in this how-to contain customer and order data. To define the access rules, you first need to set up the data structure, user roles, and GUI to maintain customer and order data.

To prepare the data structure, GUI, and example data, follow these steps:

1. Create the following domain model:

    {{< figure src="/attachments/refguide/modeling/xpath/define-access-rules-using-xpath/domain-model.png" >}}

    It contains the following:

    * **Customer** entity with the following attributes:
        * CustomerID (String)
        * Address (String)
        * ZipCode (String)
        * City (String)
    * **Order** entity with the following attributes:
        * Number (Integer)
        * Date (Date and time)
        * TotalPrice (Decimal)
        * OrderStatus (Enumeration) with the values:
            * Open
            * Processing
            * Complete
    * **Order_Customer** association
        * There is a many-to-one association from Order to Customer named "Order_Customer". This means that one customer can have multiple orders, but each order belongs to only one customer.

    For more information on creating a domain model, see [Configuring a Domain Model](/refguide/configuring-a-domain-model/).
2. Create overview and detail pages to manage the **Customer** and **Order** objects (for more information on creating these pages, see [How to Create Your First Two Overview and Detail Pages](/howto/front-end/create-your-first-two-overview-and-detail-pages/)).
3. Create menu items to access the **Order** and **Customer** overview pages (for more information on creating menu items, see [Setting Up Navigation](/refguide/setting-up-the-navigation-structure/)).
4. Set the **Security level** of you application to **Production** (for more information, see [How to Create a Secure App](/howto/security/create-a-secure-app/)).

    {{< figure src="/attachments/refguide/modeling/xpath/define-access-rules-using-xpath/app-security.png"  >}}

5. Enter *FinancialAdministrator* for the **Name** of the new user role on the **User roles** tab (for more information on adding roles, see [How to Create a Secure App](/howto/security/create-a-secure-app/):

    {{< figure src="/attachments/refguide/modeling/xpath/define-access-rules-using-xpath/add-user-role.png" class="no-border" >}}

{{% todo %}}Both module roles? Does User get created properly{{% /todo %}}

6. Give both module roles access to all your created pages, and create separate read and write access rights to all your created entities (for more information on how to set the entity access, see [How to Create a Secure App](/howto/security/create-a-secure-app/)):

    {{< figure src="/attachments/refguide/modeling/xpath/define-access-rules-using-xpath/new-access-rule.png">}}

    {{< figure src="/attachments/refguide/modeling/xpath/define-access-rules-using-xpath/page-security.png" class="no-border" >}}

7. Add new [Demo user](/refguide/demo-users/) to your application with the user role *FinancialAdministrator*:

8. Run your app locally.

9. Add the following customer data to your app:

    | CustomerID | Address | Zip code | City |
    | --- | --- | --- | --- |
    | Olav | Gedempte Zalmhaven 34 | 3050 TE | Rotterdam |
    | Tim | Kornoeljestraat 14 | 2514 RT | Den Haag |
    | Peter | Meloenstraat 123 | 2565 PE | Den Haag |
    | Harry | Emmerreklaan 25 | 1458 PE | Utrecht |

10. Add the following order data to your app:

    | Number | CustomerID | Date | Total price | Order status
    | --- | --- | --- | --- | --- |
    | 1 | Harry | 01/28/2025 | 345.00 | Open |
    | 2 | Olav | 12/30/2024 | 1234.60 | Processing |
    | 3 | Peter | 01/05/2025 | 23.60 | Open |
    | 4 | Tim | 01/04/2025 | 586.90 | Complete |
    | 5 | Olav | 01/21/2025 | 25.60 | Open |
    | 6 | Peter | 01/16/2025 | 154.00 | Complete |

## Defining the Access Rules on the Order Entity Using XPath

In the previous section, you set up a basic data structure and created some sample data. In this section, you will define the access rules on the **Order** entity so that orders can only be viewed by a financial administrator if the payment status of the order is set to **Complete**. You will do this by adding an XPath constraint to the **Order** entity for the **FinancialAdministrator** module role.

To define the access rules on the **Order** entity using XPath, follow these steps:

1. Open the **Access rules** tab for the **Order** entity:

    {{< figure src="/attachments/refguide/modeling/xpath/define-access-rules-using-xpath/access-rules.png" class="no-border" >}}

2. Double-click the column containing the **FinancialAdministrator** module role to open its properties.

3. Click **Edit…** next to the **XPath constraint**:

    {{< figure src="/attachments/refguide/modeling/xpath/define-access-rules-using-xpath/edit-xpath.png" class="no-border" >}}

4. To constrain the access of the financial administrator to only **Complete** orders, add the following **XPath**:

    ```json
    [(OrderStatus = 'Complete')]
    ```

    {{< figure src="/attachments/refguide/modeling/xpath/define-access-rules-using-xpath/define-xpath.png" class="no-border" >}}

5. Save your changes
6. Re-deploy your application.
7. When you switch to the **Financial Administrator** account, you will see that only completed orders are shown in the orders overview:

    {{< figure src="/attachments/refguide/modeling/xpath/define-access-rules-using-xpath/order-overview.png" >}}

## Read More

* [Filtering Data on an Overview Page Using XPath](/refguide/filtering-data-on-an-overview-page/)
