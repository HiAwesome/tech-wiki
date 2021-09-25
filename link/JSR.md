# JSR: Java Specification Requests

### [JSR 305: Annotations for Software Defect Detection](https://jcp.org/en/jsr/detail?id=305)

### [JSR 166: Concurrency Utilities](https://www.jcp.org/en/jsr/detail?id=166)

2.1 Please describe the proposed Specification:

This JSR has aims analogous to those of the JDK1.2 Collections package:

1. To standardize a simple, extensible framework that organizes commonly used utilities into a small enough package to be readily learnable by users and maintainable by developers.

2. To provide some high-quality implementations.

The package will consist of interfaces and classes that tend to be useful across diverse programming styles and applications. These classes include:

Atomic variables.

Special-purpose locks, barriers, semaphores and condition variables.

Queues and related collections designed for multithreaded use.

Thread pools and custom execution frameworks.

We will also examine related support in the core language and libraries.

Note that these are entirely distinct from transactional concurrency control frameworks used in J2EE. (However, they will be useful to those creating such frameworks.)

2.3 What need of the Java community will be addressed by the proposed specification?

Low level threading primitives, such as synchronized blocks, Object.wait and Object.notify, are insufficient for many programming tasks. As a result, application programmers are often forced to implement their own higher level concurrency facilities. This results in enormous duplication of effort. Further, such facilities are notoriously hard to get right and even more difficult to optimize. The concurrency facilities written by application programmers are often incorrect or inefficient. Offering a standard set of concurrency utilities will ease the task of writing a wide variety of multithreaded applications and generally improve the quality of the applications that use them.

2.4 Why isn't this need met by existing specifications?

Currently, developers can use only the concurrency control constructs provided in the Java language itself. These are too low level for some applications, and are incomplete for others.

2.5 Please give a short description of the underlying technology or technologies:

The vast majority of the package will be implemented on top of lower-level Java constructs. However, there are a few critical JVM/language enhancements surrounding atomicity and monitors necessary to obtain efficient and correct semantics.

### [JSR 303: Bean Validation](https://www.jcp.org/en/jsr/detail?id=303)

2.1 Please describe the proposed Specification:

Validating data is a common task that is copied in many different layers of an application, from the presentation tier to the persistentce layer. Many times the exact same validations will have to be implemented in each separate validation framework, proving time consuming and error-prone. To prevent having to re-implement these validations at each layer, many developers will bundle validations directly into their classes, cluttering them with copied validation code that is, in fact, meta-data about the class itself.

This JSR will define a meta-data model and API for JavaBean validation. The default meta-data source will be annotations, with the abilty to override and extend the meta-data through the use of XML validation descriptors. It is expected that the common cases will be easily accomplished using the annotations, while more complex validations or context-aware validation configuration will be available in the XML validation descriptors.

The validation API developed by this JSR will not be specific to any one tier or programming model. It will specifically not be tied to either the web tier or the persistence tier, and will be available for both server-side application programming, as well as rich client Swing application developers. This API is seen as a general extension to the JavaBeans object model, and as such is expected to be used as a core component in other specifications, such as JSF, JPA, and Bean Binding.

2.5 What need of the Java community will be addressed by the proposed specification?

This work will address the need of the Java community for standardized validation meta-data and a standard validation API. This meta-data will be valuable across a number of application domains, from Swing desktop applications to web applications and the persistence layer.

2.6 Why isn't this need met by existing specifications?

Existing specifications touch on validation or offer domain-specific validation, but none offer a reusable validation framework which can be used across application domains.

2.7 Please give a short description of the underlying technology or technologies:

This JSR, like many in the post-JDK 1,5 world, seeks to define meta-data about objects using a combination of annotations and XML descriptors. In addition this JSR will provide a runtime validation framework to check JavaBean property values against the validations defined for them.
