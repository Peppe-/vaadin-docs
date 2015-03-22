---
layout: page
title: Migrating from Vaadin 6
---
*This appendix is an incomplete draft. For the most current version,
please see the on-line edition available at <http://vaadin.com/book> and
the Vaadin 7 Migration Guide at
[http://dev.vaadin.com/wiki/Vaadin7/MigrationGuide](#).*

## Overview


The Vaadin 7 API is more clearly for the web. Earlier many web concepts
were more abstracted from the developer, while now they are more easily
accessible. For example, the init() method receives as a parameter HTTP
request data, which was earlier more difficult to obtain. Also the new
central concept of `` is for web use where Vaadin applications are often
embedded in web pages.

## The Role of `` 


The "main window" concept in Vaadin 6 is replaced with *UI*. A UI is in
most cases what "main window" used to be in Vaadin 6, and in most code
you can simply change `Window` to ``.

Even more visibly, a Vaadin application does not any longer extend
`Application`, but ``.

Consider a Vaadin 6 application:
{% highlight java linenos %}
public class MyApplication extends Application {
    @Override
    public void init() {
        mainWindow = new Window("Myportlet Application");
        setMainWindow(mainWindow);

        mainWindow.addComponent(new Label("Hello, world!"));
        ...
    }
}
{% endhighlight %}
In Vaadin 7, the equivalent is:
{% highlight java linenos %}
public class MyUI extends UI {
    @Override
    protected void init(WrappedRequest request) {
        addComponent(new Label("Hello, world!"));
        ...
    }
}
{% endhighlight %}
The new `WrappedRequest` contains request data received in the initial
request, which was previously only available by implementing
HttpServletRequestListener in the application class.
