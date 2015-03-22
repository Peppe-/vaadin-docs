---
layout: page
title: Client-Side Widgets
order: 14
---

This chapter gives basic documentation on the use of the Vaadin
client-side framework for the purposes of creating client-side
applications and writing your own widgets.

*Currently, we only give a brief introduction to the topic in this
chapter. Please refer to the GWT documentation for a more complete
treatment of the GWT widgets.*

Overview {#clientsidewidgets.overview}
========

The Vaadin client-side API is based on the Google Web Toolkit. It
involves *widgets* for representing the user interface as Java objects,
which are rendered as a HTML DOM in the browser. Events caused by user
interaction with the page are delegated to event handlers, where you can
implement your UI logic.

In general, the client-side widgets come in two categories - basic GWT
widgets and Vaadin-specific widgets. The library includes *connectors*
for integrating the Vaadin-specific widgets with the server-side
components, thereby enabling the server-side development model of
Vaadin. The integration is described in ?.

The layout of the client-side UI is managed with *panel* widgets, which
correspond in their function with layout components in the Vaadin
server-side API.

In addition to the rendering API, the client-side API includes
facilities for making HTTP requests, logging, accessibility,
internationalization, and testing.

For information about the basic GWT framework, please refer to
[https://developers.google.com/web-toolkit/overview](#).

GWT Widgets {#clientsidewidgets.gwt}
===========

GWT widgets are user interface elements that are rendered as HTML.
Rendering is done either by manipulating the HTML Document Object Model
(DOM) through the lower-level DOM API, or simply by injecting the HTML
with setInnerHTML(). The layout of the user interface is managed using
special panel widgets.

For information about the basic GWT widgets, please refer to the GWT
Developer's Guide at
[https://developers.google.com/web-toolkit/doc/latest/DevGuideUi](#).

Vaadin Widgets {#clientsidewidgets.vaadin}
==============

Vaadin comes with a number of Vaadin-specific widgets in addition to the
GWT widgets, some of which you can use in pure client-side applications.
The Vaadin widgets have somewhat different feature set from the GWT
widgets and are foremost intended for integration with the server-side
components, but some may prove useful for client-side applications as
well.

    public class MyEntryPoint implements EntryPoint {
        @Override
        public void onModuleLoad() {
            // Add a Vaadin button
            VButton button = new VButton();
            button.setText("Click me!");
            button.addClickHandler(new ClickHandler() {
                @Override
                public void onClick(ClickEvent event) {
                    mywidget.setText("Clicked!");
                }
            });
            
            RootPanel.get().add(button);
        }
    }

Grid {#clientsidewidgets.grid}
====

The `Grid` widget is the client-side counterpart for the server-side
`Grid` component described in ?.

The client-side API is almost identical to the server-side API, so its
documentation is currently omitted here and we refer you to the API
documentation. In the following, we go through some customization
features of `Grid`.

Renderers {#clientsidewidgets.grid.renderers}
---------

As described in ?, renderers draw the visual representation of data
values on the client-side. They implement Renderer interface and its
render() method. The method gets a reference to the element of the grid
cell, as well as the data value to be rendered. An implementation needs
to modify the element as needed.

For example, `TextRenderer` is implemented simply as follows:

    public class TextRenderer implements Renderer<String> {
        @Override
        public void render(RendererCellReference cell,
                           String text) {
            cell.getElement().setInnerText(text);
        }
    }

The server-side renderer API should extend `AbstractRenderer` or
`ClickableRenderer` with the data type accepted by the renderer. The
data type also must be given for the superclass constructor.

    public class TextRenderer extends AbstractRenderer<String> {
        public TextRenderer() {
            super(String.class);
        }
    }

The client-side and server-side renderer need to be connected with a
connector extending from `AbstractRendererConnector`.

    @Connect(com.vaadin.ui.renderer.TextRenderer.class)
    public class TextRendererConnector
           extends AbstractRendererConnector<String> {
        @Override
        public TextRenderer getRenderer() {
            return (TextRenderer) super.getRenderer();
        }
    }

Renderers can have parameters, for which normal client-side
communication of extension parameters can be used. Please see the
implementations of different renderers for examples.
