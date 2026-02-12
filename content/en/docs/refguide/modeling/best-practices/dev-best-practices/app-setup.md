---
title: "App Setup"
linktitle: "App Setup Best Practices"
url: /refguide/app-setup/
description: "Explains best practices to use when setting up Mendix apps."
no_list: false
description_list: true
---

## App Setup

### The Application Development Language

The language that will be used to develop the application should be determined upfront. This way you have one language for modules, entities, microflows, pages, etc. The preferred language for development is English.

There are some reasons why certain parts of an application may use another language. The main reason to make an exception would be within the domain model of an integration module. For example, when the source data model is in another language already.

For more information, see [Translating Your App Content](/refguide/translate-your-app-content/).

### App Name

Every app is named when it is created. Make sure you use a logical name that allows you to easily identify the application. You will probably create more apps in the future, and will want to be able to recognize this app. Mendix recommends leaving out dates or Mendix version numbers in the app name, since that information can be captured and extracted in a different way.

### Configurations

Every app has at least one configuration, but it may have many. Every app starts with a single configuration called **default**. When you work with multiple people on an application it is beneficial to create multiple configurations. When doing so, Mendix recommends using relevant names for those configurations, like the name of the developer or the app's purpose, like **Test** or **Acceptance**. Beware that the database passwords defined in the configuration will be visible to other team members, so be careful with using personal passwords you'd like to keep secret.

### User Roles

The [user roles](/refguide/user-roles/) should have logical names that reflect the different types of users that will use the application. The user roles are singular and use an UpperCamelCase notation, like **FunctionalAdministrator**. User roles are mostly defined in English, but there is an option to name these in a different language, since the user role is visible in the front end.

Each user role should correspond to only one module role per module. In other words, a user role should not map to multiple module roles within the same module. This helps to keep the number of applicable module roles for a user to a minimum, which reduces complexity in understanding the security model and reduces the performance impact of complex security rules.

### Passwords and Other Secrets

Always store secret information in a safe place. A safe place is the database. Use the [Encryption](https://marketplace.mendix.com/link/component/1011) module to encrypt, store, retrieve, and decrypt the information.

You can also store [private constants](/refguide/configuration/#constants) in configurations. These are encrypted and stored on your local machine so will not be shared with others.

Using either the default value of a constant or the project's shared configuration settings is unsafe. Both these places are readable by others and visible in the version management copies. 