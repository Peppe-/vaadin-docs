---
layout: page
title: Preface
order: 2
---
This book provides an overview of the Vaadin Framework and covers the
most important topics that you might encounter while developing
applications with it. A more detailed documentation of the individual
classes, interfaces, and methods is given in the Vaadin API Reference.

The book has grown greatly after its first edition, until it became too
thick to fit any standard pocket. To accommodate all the content also in
the printed edition, it has now been split into two volumes, the first
one including the topics that you need to get started with Vaadin.

This edition mostly covers Vaadin 7.3 released in 2014. The most notable
feature in the release is the new and highly customizable Valo theme.

In addition to the changes in the core framework, this edition features
documentation for the TestBench 4 and TouchKit 4 add-ons, which are not
yet released at the time of writing. The updated chapters are based on
prerelease versions of the add-ons, so the final releases may include
some changes.

Writing this manual is an ongoing work and it is rarely completely
up-to-date with the quick-evolving product. Some features may not be
included in this book yet. For the most current version, please see the
on-line edition available at <http://vaadin.com/book>. You can also find
PDF and EPUB versions of the book there. You may find the other versions
more easily searchable than the printed book. The index in the book is
incomplete and will be expanded later. The web edition also has some
additional technical content, such as some example code and additional
sections that you may need when actually doing development. The purpose
of the slightly abridged print edition is more to be an introductionary
textbook to Vaadin, and still fit in your pocket.

Also, many Vaadin 7 features are showcased as mini-tutorials, which are
available in the Vaadin Wiki at
[https://vaadin.com/wiki/-/wiki/Main/Vaadin+7](#).

This book is intended for software developers who use, or are
considering to use, Vaadin to develop web applications.

The book assumes that you have some experience with programming in Java,
but if not, it is at least as easy to begin learning Java with Vaadin as
with any other UI framework. No knowledge of AJAX is needed as it is
well hidden from the developer.

You may have used some desktop-oriented user interface frameworks for
Java, such as AWT, Swing, or SWT, or a library such as Qt for C++. Such
knowledge is useful for understanding the scope of Vaadin, the
event-driven programming model, and other common concepts of UI
frameworks, but not necessary.

If you do not have a web graphics designer at hand, knowing the basics
of HTML and CSS can help so that you can develop presentation themes for
your application. A brief introduction to CSS is provided. Knowledge of
Google Web Toolkit (GWT) may be useful if you develop or integrate new
client-side components.

The Book of Vaadin gives an introduction to what Vaadin is and how you
use it to develop web applications.

Volume 1

?

:   The chapter gives an introduction to the application architecture
    supported by Vaadin, the core design ideas behind the framework, and
    some historical background.

?

:   This chapter gives practical instructions for installing Vaadin and
    the reference toolchain, including the Vaadin Plugin for Eclipse,
    how to run and debug the demos, and how to create your own
    application project in the Eclipse IDE.

?

:   This chapter gives an introduction to the architecture of Vaadin and
    its major technologies, including AJAX, Google Web Toolkit, and
    event-driven programming.

?

:   This chapter gives all the practical knowledge required for creating
    applications with Vaadin, such as window management, application
    lifecycle, deployment in a servlet container, and handling events,
    errors, and resources.

?

:   This chapter gives the basic usage documentation for all the
    (non-layout) user interface components in Vaadin and their most
    significant features. The component sections include examples for
    using each component, as well as for styling with CSS/Sass.

?

:   This chapter describes the layout components, which are used for
    managing the layout of the user interface, just like in any desktop
    application frameworks.

Volume 2:

?

:   This chapter gives an introduction to Cascading Style Sheets (CSS)
    and Sass and explains how you can use them to build custom visual
    themes for your application.

?

:   This chapter gives an overview of the built-in data model of Vaadin,
    consisting of properties, items, and containers.

?

:   This chapter gives documentation for the SQLContainer, which allows
    binding Vaadin components to SQL queries.

?

:   This chapter gives instructions for using the visual editor for
    Eclipse, which is included in the Vaadin Plugin for the Eclipse IDE.

?

:   This chapter provides many special topics that are commonly needed
    in applications, such as opening new browser windows, embedding
    applications in regular web pages, low-level management of
    resources, shortcut keys, debugging, etc.

?

:   This chapter describes the development of Vaadin applications as
    portlets which you can deploy to any portal supporting Java Portlet
    API 2.0 (JSR-286). The chapter also describes the special support
    for Liferay and the Control Panel, IPC, and WSRP add-ons.

?

:   This chapter gives an introduction to creating and developing
    client-side applications and widgets, including installation,
    compilation, and debugging.

?

:   This chapter describes how to develop client-side applications and
    how to integrate them with a back-end service.

?

:   This chapter describes the built-in widgets (client-side components)
    available for client-side development. The built-in widgets include
    Google Web Toolkit widgets as well as Vaadin widgets.

?

:   This chapter describes how to integrate client-side widgets with
    their server-side counterparts for the purpose of creating new
    server-side components. The chapter also covers integrating
    JavaScript components.

<!-- -->

?

:   This chapter gives instructions for downloading and installing
    add-on components from the Vaadin Directory.

?

:   This chapter documents the use of the Vaadin Charts add-on component
    for interactive charting with many diagram types. The add-on
    includes the Chart and Timeline components.

?

:   This chapter gives documentation of the JPAContainer add-on, which
    allows binding Vaadin components directly to relational and other
    databases using Java Persistence API (JPA).

?

:   This chapter gives examples and reference documentation for using
    the Vaadin TouchKit add-on for developing mobile applications.

?

:   This chapter gives the complete documentation of using the Vaadin
    TestBench tool for recording and executing user interface regression
    tests of Vaadin applications.

The Vaadin websites offer plenty of material that can help you
understand what Vaadin is, what you can do with it, and how you can do
it.

Demo Applications

:   The most important demo application for Vaadin is the Sampler, which
    demonstrates the use of all basic components and features. You can
    run it on-line at [http://demo.vaadin.com/](#) or download it as a
    WAR from the [Vaadin download page](#).

    Most of the code examples in this book and many others can be found
    online at [http://demo.vaadin.com/book-examples-vaadin7/book/](#).

Cheat Sheet

:   The two-page cheat sheet illustrates the basic relationship
    hierarchy of the user interface and data binding classes and
    interfaces. You can download it at [http://vaadin.com/book](#).

Refcard

:   The six-page DZone Refcard gives an overview to application
    development with Vaadin. It includes a diagram of the user interface
    and data binding classes and interfaces. You can find more
    information about it at [https://vaadin.com/refcard](#).

Address Book Tutorial

:   The Address Book is a sample application accompanied with a tutorial
    that gives detailed step-by-step instructions for creating a
    real-life web application with Vaadin. You can find the tutorial
    from the product website.

Developer's Website

:   Vaadin Developer's Site at [http://dev.vaadin.com/](#) provides
    various online resources, such as the ticket system, a development
    wiki, source repositories, activity timeline, development
    milestones, and so on.

    The wiki provides instructions for developers, especially for those
    who wish to check-out and compile Vaadin itself from the source
    repository. The technical articles deal with integration of Vaadin
    applications with various systems, such as JSP, Maven, Spring,
    Hibernate, and portals. The wiki also provides answers to Frequently
    Asked Questions.

Online Documentation

:   You can read this book online at [http://vaadin.com/book](#). Lots
    of additional material, including technical HOWTOs, answers to
    Frequently Asked Questions and other documentation is also available
    on [Vaadin web-site](#).

Stuck with a problem? No need to lose your hair over it, the Vaadin
Framework developer community and the Vaadin company offer support to
all of your needs.

Community Support Forum

:   You can find the user and developer community forum at
    [http://vaadin.com/forum](#). Please use the forum to discuss any
    problems you might encounter, wishes for features, and so on. The
    answer to your problems may already lie in the forum archives, so
    searching the discussions is always the best way to begin.

Report Bugs

:   If you have found a possible bug in Vaadin, the demo applications,
    or the documentation, please report it by filing a ticket at the
    Vaadin developer's site at [http://dev.vaadin.com/](#). You may want
    to check the existing tickets before filing a new one. You can make
    a ticket to make a request for a new feature as well, or to suggest
    modifications to an existing feature.

Commercial Support

:   Vaadin offers full commercial support and training services for the
    Vaadin Framework and related products. Read more about the
    commercial products at [http://vaadin.com/pro](#) for details.

Marko Grönroos is a professional writer and software developer working
at Vaadin Ltd in Turku, Finland. He has been involved in web application
development since 1994 and has worked on several application development
frameworks in C, C++, and Java. He has been active in many open source
software projects and holds an M.Sc. degree in Computer Science from the
University of Turku.

Much of the book is the result of close work within the development team
at Vaadin Ltd. Joonas Lehtinen, CEO of Vaadin Ltd, wrote the first
outline of the book, which became the basis for the first two chapters.
Since then, Marko Grönroos has become the primary author and editor. The
development team has contributed several passages, answered numerous
technical questions, reviewed the manual, and made many corrections.

The contributors are (in rough chronological order):

-   Joonas Lehtinen
-   Jani Laakso
-   Marko Grönroos
-   Jouni Koivuviita
-   Matti Tahvonen
-   Artur Signell
-   Marc Englund
-   Henri Sara
-   Jonatan Kronqvist
-   Mikael Grankvist (TestBench)
-   Teppo Kurki (SQLContainer)
-   Tomi Virtanen (Calendar)
-   Risto Yrjänä (Calendar)
-   John Ahlroos (Timeline)
-   Petter Holmström (JPAContainer)
-   Leif Åstrand

Vaadin Ltd is a Finnish software company specializing in the design and
development of Rich Internet Applications. The company offers planning,
implementation, and support services for the software projects of its
customers, as well as sub-contract software development. Vaadin
Framework, previously known as IT Mill Toolkit, is the flagship open
source product of the company, for which it provides commercial
development and support services.
