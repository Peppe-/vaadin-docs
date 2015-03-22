---
layout: page
title: Client-Side Applications
order: 13
---

This chapter describes how to develop client-side Vaadin applications.

*Currently, we only give a brief introduction to the topic. Please refer
to the GWT documentation for a more complete treatment of the many GWT
features.*

Overview {#clientsideapp.overview}
========

Vaadin allows developing client-side modules that run in the browser.
Client-side modules can use all the GWT widgets and some Vaadin-specific
widgets, as well as the same themes as server-side Vaadin applications.
Client-side applications run in the browser, even with no further server
communications. When paired with a server-side service to gain access to
data storage and server-side business logic, client-side applications
can be considered "fat clients", in comparison to the "thin client"
approach of the server-side Vaadin applications. The services can use
the same back-end services as server-side Vaadin applications. Fat
clients are useful for a range of purposes when you have a need for
highly responsive UI logic, such as for games or for serving a huge
number of clients with possibly stateless server-side code.

![Client-Side Application
Architecture](img/clientsideapp/clientsideapp-architecture-lo.png)

A client-side application is defined as a *module*, which has an
*entry-point* class. Its onModuleLoad() method is executed when the
JavaScript of the compiled module is loaded in the browser.

Consider the following client-side application:

    public class HelloWorld implements EntryPoint {
        @Override
        public void onModuleLoad() {
            RootPanel.get().add(new Label("Hello, world!"));
        }
    }

The user interface of a client-side application is built under a HTML
*root element*, which can be accessed by RootPanel.get(). The purpose
and use of the entry-point is documented in more detail in ?. The user
interface is built from *widgets* hierarchically, just like with
server-side Vaadin UIs. The built-in widgets and their relationships are
catalogued in ?. You can also use many of the widgets in Vaadin add-ons
that have them, or make your own.

A client-side module is defined in a *module descriptor*, as described
in ?. A module is compiled from Java to JavaScript using the Vaadin
Compiler, of which use was described in ?. The ? in this chapter gives
further information about compiling client-side applications. The
resulting JavaScript can be loaded to any web page, as described in ?.

The client-side user interface can be built declaratively using the
included *UI Binder*.

The servlet for processing RPC calls from the client-side can be
generated automatically using the included compiler.

Even with regular server-side Vaadin applications, it may be useful to
provide an off-line mode if the connection is closed. An off-line mode
can persist data in a local store in the browser, thereby avoiding the
need for server-side storage, and transmit the data to the server when
the connection is again available. Such a pattern is commonly used with
Vaadin TouchKit.

Client-Side Module Entry-Point {#clientsideapp.entrypoint}
==============================

A client-side application requires an *entry-point* where the execution
starts, much like the init() method in server-side Vaadin UIs.

Consider the following application:

    package com.example.myapp.client;

    import com.google.gwt.core.client.EntryPoint;
    import com.google.gwt.event.dom.client.ClickEvent;
    import com.google.gwt.event.dom.client.ClickHandler;
    import com.google.gwt.user.client.ui.RootPanel;
    import com.vaadin.ui.VButton;

    public class MyEntryPoint implements EntryPoint {
        @Override
        public void onModuleLoad() {
            // Create a button widget
            Button button = new Button();
            button.setText("Click me!");
            button.addClickHandler(new ClickHandler() {
                @Override
                public void onClick(ClickEvent event) {
                    mywidget.setText("Hello, world!");
                }
            });
            RootPanel.get().add(button);
        }
    }

Before compiling, the entry-point needs to be defined in a module
descriptor, as described in the next section.

Module Descriptor {#clientsideapp.entrypoint.descriptor}
-----------------

The entry-point of a client-side application is defined, along with any
other configuration, in a client-side module descriptor, described in ?.
The descriptor is an XML file with suffix `.gwt.xml`.

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE module PUBLIC
    "-//Google Inc.//DTD Google Web Toolkit 1.7.0//EN"
    "http://google-web-toolkit.googlecode.com/svn/tags/1.7.0/distro-source/core/src/gwt-module.dtd">
    <module>
        <!-- Builtin Vaadin and GWT widgets -->
        <inherits name="com.vaadin.Vaadin" />

        <!-- The entry-point for the client-side application -->
        <entry-point class="com.example.myapp.client.MyEntryPoint"/>
    </module>

You might rather want to inherit the `com.google.gwt.user.User` to get
just the basic GWT widgets, and not the Vaadin-specific widgets and
classes, most of which are unusable in pure client-side applications.

You can put static resources, such as images or CSS stylesheets, in a
`public` folder (not a Java package) under the folder of the descriptor
file. When the module is compiled, the resources are copied to the
output folder. Normally in pure client-side application development, it
is easier to load them in the HTML host file or in a `ClientBundle` (see
GWT documentation), but these methods are not compatible with
server-side component integration, if you use the resources for that
purpose as well.

Compiling and Running a Client-Side Application {#clientsideapp.compiling}
===============================================

*Compilation of client-side modules other than widget sets with the
Vaadin Plugin for Eclipse has recent changes and limitations at the time
of writing of this edition and the information given here may not be
accurate.*

The application needs to be compiled into JavaScript to run it in a
browser. For deployment, and also initially for the first time when
running the Development Mode, you need to do the compilation with the
Vaadin Client Compiler, as described in ?.

During development, it is easiest to use the SuperDevMode, which also
quickly launching the client-side code and also allows debugging. See ?
for more details.

Loading a Client-Side Application {#clientsideapp.loading}
=================================

You can load the JavaScript code of a client-side application in an HTML
*host page* by including it with a `<script>` tag, for example as
follows:

    <html xmlns="http://www.w3.org/1999/xhtml">
      <head>
        <meta http-equiv="Content-Type"
              content="text/html; charset=UTF-8" />

        <title>Embedding a Vaadin Application in HTML Page</title>

        <!-- Load the Vaadin style sheet -->
        <link rel="stylesheet"
              type="text/css"
              href="/myproject/VAADIN/themes/reindeer/legacy-styles.css"/>
      </head>

      <body>
        <h1>A Pure Client-Side Application</h1>
        
        <script type="text/javascript" language="javascript"
                src="clientside/com.example.myapp.MyModule/
                     com.example.myapp.MyModule.nocache.js">
        </script>
      </body>
    </html>

The JavaScript module is loaded in a `<script>` element. The `src`
parameter should be a relative link from the host page to the compiled
JavaScript module.

If the application uses any supplementary Vaadin widgets, and not just
core GWT widgets, you need to include the Vaadin theme as was done in
the example. The exact path to the style file depends on your project
structure - the example is given for a regular Vaadin application where
themes are contained in the `VAADIN` folder in the WAR.

In addition to CSS and scripts, you can load any other resources needed
by the client-side application in the host page.
