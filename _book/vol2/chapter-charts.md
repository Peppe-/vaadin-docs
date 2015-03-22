---
layout: page
title: Vaadin Charts
order: 17
---

This chapter provides the documentation for the Vaadin Charts add-on.

Overview {#charts.overview}
========

Vaadin Charts is a feature-rich interactive charting library for Vaadin.
It provides a `Chart` and a `Timeline` component. The `Chart` can
visualize one- and two-dimensional numeric data in many available chart
types. The charts allow flexible configuration of all the chart elements
as well as the visual style. The library includes a number of built-in
visual themes, which you can extend further. The basic functionalities
allow the user to interact with the chart elements in various ways, and
you can define custom interaction with click events. The `Timeline` is a
specialized component for visualizing time series, and is described in
?.

![Vaadin Charts](img/charts/charts-overview.png)

The data displayed in a chart can be one- or two dimensional tabular
data, or scatter data with free X and Y values. Data displayed in range
charts has minimum and maximum values instead of singular values.

This chapter covers the basic use of Vaadin Charts and the chart
configuration. For detailed documentation of the configuration
parameters and classes, please refer to the JavaDoc API documentation of
the library.

In the following basic example, which we study further in ?, we
demonstrate how to display one-dimensional data in a column graph and
customize the X and Y axis labels and titles.

    Chart chart = new Chart(ChartType.BAR);
    chart.setWidth("400px");
    chart.setHeight("300px");
            
    // Modify the default configuration a bit
    Configuration conf = chart.getConfiguration();
    conf.setTitle("Planets");
    conf.setSubTitle("The bigger they are the harder they pull");
    conf.getLegend().setEnabled(false); // Disable legend

    // The data
    ListSeries series = new ListSeries("Diameter");
    series.setData(4900,  12100,  12800,
                   6800,  143000, 125000,
                   51100, 49500);
    conf.addSeries(series);

    // Set the category labels on the axis correspondingly
    XAxis xaxis = new XAxis();
    xaxis.setCategories("Mercury", "Venus",   "Earth",
                        "Mars",    "Jupiter", "Saturn",
                        "Uranus",  "Neptune");
    xaxis.setTitle("Planet");
    conf.addxAxis(xaxis);

    // Set the Y axis title
    YAxis yaxis = new YAxis();
    yaxis.setTitle("Diameter");
    yaxis.getLabels().setFormatter(
      "function() {return Math.floor(this.value/1000) + \' Mm\';}");
    yaxis.getLabels().setStep(2);
    conf.addyAxis(yaxis);
            
    layout.addComponent(chart);

The resulting chart is shown in ?.

![Basic Chart Example](img/charts/charts-basicexample.png)

Vaadin Charts is based on Highcharts JS, a charting library written in
JavaScript.

Vaadin Charts is a commercial product licensed under the CVAL License
(Commercial Vaadin Add-On License). A license needs to be purchased for
all use, including web deployments as well as intranet use. Using Vaadin
Charts does not require purchasing a separate Highcharts JS license.

The commercial licenses can be purchased from the [Vaadin Directory](#),
where you can also find the license details and download the Vaadin
Charts.

Installing Vaadin Charts {#charts.installing}
========================

As with most Vaadin add-ons, you can install Vaadin Charts as a Maven or
Ivy dependency in your project, or from an installation package. For
general instructions on installing add-ons, please see ?.

Vaadin Charts requires Vaadin 7.3 or later.

Using Vaadin Charts requires a license key, which you must install
before compiling the widget set. The widget set must be compiled after
setting up the dependency or library JARs.

Maven Dependency {#charts.installing.maven}
----------------

The Maven dependency for Vaadin TestBench is as follows:

    <dependency>
        <groupId>com.vaadin.addon</groupId>
        <artifactId>vaadin-charts</artifactId>
        <version>2.0.0</version>
    </dependency>

You also need to define the Vaadin add-ons repository if not already
defined:

    <repository>
       <id>vaadin-addons</id>
       <url>http://maven.vaadin.com/vaadin-addons</url>
    </repository>

Ivy Dependency {#charts.installing.ivy}
--------------

The Ivy dependency, to be defined in `ivy.xml`, would be as follows:

    <dependency org="com.vaadin" name="vaadin-charts"
                rev="2.0" />

It is generally recommended to use a fixed version number, but you can
also use `latest.release` to get the latest release.

Installing License Key {#charts.installing.license}
----------------------

You need to install a license key before compiling the widget set. The
license key is checked during widget set compilation, so you do not need
it when deploying the application.

You can purchase Vaadin Charts or obtain a free trial key from the
[Vaadin Charts download page](#) in Vaadin Directory. You need to
register in Vaadin Directory to obtain the key.

![Obtaining License Key from Vaadin
Directory](img/testbench/screenshots/directory-license-key.png)

To install the license key in a development workstation, you can copy
and paste it verbatim to a `.vaadin.charts.developer.license` file in
your home directory. For example, in Linux and OS X:

    $ echo "L1cen5e-c0de" > ~/.vaadin.charts.developer.license

You can also pass the key as a system property to the widget set
compiler, usually with a `-D` option. For example, on the command-line:

    $ java -Dvaadin.charts.developer.license=L1cen5e-c0de ...

See [the AGPL license key installation instructions](#) for more
details.

### Passing License Key in Different Environments {#charts.installing.license.environments}

How you actually pass the parameter to the widget set compiler depends
on the development environment and the build system that you use to
compile the widget set. Below are listed a few typical environments:

Eclipse IDE

:   To install the license key for all projects, select <span
    class="menuchoice">Window \> Preferences</span> and navigate to the
    <span class="menuchoice">Java \> Installed JREs</span> section.
    Select the JRE version that you use for the application and click
    Edit. In the Default VM arguments, give the `-D` expression as shown
    above.

Apache Ant

:   If compiling the project with Apache Ant, you could set the key in
    the Ant script as follows:

        <sysproperty key="vaadin.charts.developer.license"
                     value="L1cen5e-c0de"/>

    However, you should never store license keys in a source repository,
    so if the Ant script is stored in a source repository, you should
    pass the license key to Ant as a property that you then use in the
    script for the value argument of the `<sysproperty>` as follows:

        <sysproperty key="vaadin.charts.developer.license"
            value="${vaadin.charts.developer.license}"/>

    When invoking Ant from the command-line, you can pass the property
    with a `-D` parameter to Ant.

Apache Maven

:   If building the project with Apache Maven, you can pass the license
    key with a `-D` parameter to Maven:

        $ mvn -Dvaadin.charts.developer.license=L1cen5e-c0de package

Continuous Integration Systems

:   In CIS systems, you can pass the license key to build runners as a
    system property in the build configuration. However, this only
    passes it to a runner. As described above, Ant does not pass it to
    sub-processes implicitly, so you need to forward it explicitly as
    described earlier.

Basic Use {#charts.basic-use}
=========

The `Chart` is a regular Vaadin component, which you can add to a
layout. You can give the chart type in the constructor or set it later
in the chart model. A chart has a height of 400 pixels and takes full
width by default, which settings you may often need to customize.

    Chart chart = new Chart(ChartType.COLUMN);
    chart.setWidth("400px");  // 100% by default
    chart.setHeight("300px"); // 400px by default
    ...
    layout.addComponent(chart);

The chart types are described in ?. The main parts of a chart are
illustrated in ?.

![Chart Elements](img/charts/charts-elements-hi.png)

To actually display something in a chart, you typically need to
configure the following aspects:

-   Basic chart configuration
-   Configure
    plot options
    for the chart type
-   Configure one or more
    data series
    to display
-   Configure
    axes

The plot options can be configured for each data series individually, or
for different chart types in mixed-type charts.

Basic Chart Configuration {#charts.basic-use.configuration}
-------------------------

After creating a chart, you need to configure it further. At the least,
you need to specify the data series to be displayed in the
configuration.

Most methods available in the `Chart` object handle its basic Vaadin
component properties. All the chart-specific properties are in a
separate `Configuration` object, which you can access with the
getConfiguration() method.

    Configuration conf = chart.getConfiguration();
    conf.setTitle("Reindeer Kills by Predators");
    conf.setSubTitle("Kills Grouped by Counties");

The configuration properties are described in more detail in ?.

Plot Options {#charts.basic-use.plotoptions}
------------

Many chart settings can be configured in the *plot options* of the chart
or data series. Some of the options are chart type specific, as
described later for each chart type, while many are shared.

For example, for line charts, you could disable the point markers as
follows:

    // Disable markers from lines
    PlotOptionsLine plotOptions = new PlotOptionsLine();
    plotOptions.setMarker(new Marker(false));
    conf.setPlotOptions(plotOptions);

You can set the plot options for the entire chart or for each data
series separately, allowing also mixed-type charts, as described in ?.

The shared plot options are described in ?.

Chart Data Series {#charts.basic-use.data}
-----------------

The data displayed in a chart is stored in the chart configuration as a
list of `Series` objects. A new data series is added in a chart with the
addSeries() method.

    ListSeries series = new ListSeries("Diameter");
    series.setData(4900,  12100,  12800,
                   6800,  143000, 125000,
                   51100, 49500);
    conf.addSeries(series);

The data can be specified with a number of different series types
`DataSeries`, `ListSeries`, `AreaListSeries`, `RangeSeries`, and
`ContainerDataSeries`.

Data point features, such as color and size, can be defined in the
versatile `DataSeries`, which contains `DataSeriesItem` items. Special
chart types, such as box plots and 3D scatter charts require using their
own special data point type.

The data series configuration is described in more detail in ?.

Axis Configuration {#charts.basic-use.axis}
------------------

One of the most common tasks for charts is customizing its axes. At the
least, you usually want to set the axis titles. Usually you also want to
specify labels for data values in the axes.

When an axis is categorical rather than numeric, you can define category
labels for the items. They must be in the same order and the same number
as you have values in your data series.

    XAxis xaxis = new XAxis();
    xaxis.setCategories("Mercury", "Venus",   "Earth",
                        "Mars",    "Jupiter", "Saturn",
                        "Uranus",  "Neptune");
    xaxis.setTitle("Planet");
    conf.addxAxis(xaxis);

Formatting of numeric labels can be done with JavaScript expressions,
for example as follows:

    // Set the Y axis title
    YAxis yaxis = new YAxis();
    yaxis.setTitle("Diameter");
    yaxis.getLabels().setFormatter(
      "function() {return Math.floor(this.value/1000) + \'Mm\';}");
    yaxis.getLabels().setStep(2);
    conf.addyAxis(yaxis);

Displaying Multiple Series {#charts.basic-use.two-dimensional}
--------------------------

The simplest data, which we saw in the examples earlier in this chapter,
is one-dimensional and can be represented with a single data series.
Most chart types support multiple data series, which are used for
representing two-dimensional data. For example, in line charts, you can
have multiple lines and in column charts the columns for different
series are grouped by category. Different chart types can offer
alternative display modes, such as stacked columns. The legend displays
the symbols for each series.

    // The data
    // Source: V. Maijala, H. Norberg, J. Kumpula, M. Nieminen
    // Calf production and mortality in the Finnish
    // reindeer herding area. 2002.
    String predators[] = {"Bear", "Wolf", "Wolverine", "Lynx"};
    int kills[][] = {        // Location:
            {8,   0,  7, 0}, // Muddusjarvi
            {30,  1, 30, 2}, // Ivalo
            {37,  0, 22, 2}, // Oraniemi
            {13, 23,  4, 1}, // Salla
            {3,  10,  9, 0}, // Alakitka
    };    

    // Create a data series for each numeric column in the table
    for (int predator = 0; predator < 4; predator++) {
        ListSeries series = new ListSeries();
        series.setName(predators[predator]);
        
        // The rows of the table
        for (int location = 0; location < kills.length; location++)
            series.addData(kills[location][predator]);
        conf.addSeries(series);
    }

The result for both regular and stacked column chart is shown in ?.
Stacking is enabled with setStacking() in `PlotOptionsColumn`.

![Multiple Series in a Chart](img/charts/charts-twodimensional.png)

Mixed Type Charts {#charts.basic-use.mixed}
-----------------

You can enable mixed charts by setting the chart type in the
`PlotOptions` object for a data series, which overrides the default
chart type set in the `Chart` object. You can also make color and other
settings for the series in the plot options.

For example, to get a line chart, you need to use `PlotOptionsLine`.

    // A data series as column graph
    DataSeries series1 = new DataSeries();
    PlotOptionsColumn options1 = new PlotOptionsColumn();
    options1.setFillColor(SolidColor.BLUE);
    series1.setPlotOptions(options1);
    series1.setData(4900,  12100,  12800,
        6800,  143000, 125000, 51100, 49500);
    conf.addSeries(series1);

    // A data series as line graph
    ListSeries series2 = new ListSeries("Diameter");
    PlotOptionsLine options2 = new PlotOptionsLine();
    options2.setLineColor(SolidColor.RED);
    series2.setPlotOptions(options2);
    series2.setData(4900,  12100,  12800,
        6800,  143000, 125000, 51100, 49500);
    conf.addSeries(series2);

In the above case, where we set the chart type for each series, the
overall chart type is irrelevant.

3D Charts {#charts.basic-use.3d}
---------

Most chart types can be made 3-dimensional by adding 3D options to the
chart. You can rotate the charts, set up the view distance, and define
the thickness of the chart features, among other things. You can also
set up a 3D axis frame around a chart.

![3D Charts](img/charts/charts-3d-pie.png)

### 3D Options {#charts.basic-use.3d.options}

3D view has to be enabled in the `Options3d` configuration, along with
other parameters. Minimally, to have some 3D effect, you need to rotate
the chart according to the *alpha* and *beta* parameters.

Let us consider a basic scatter chart for an example. The basic
configuration for scatter charts is described elsewhere, but let us look
how to make it 3D.

    Chart chart = new Chart(ChartType.SCATTER);
    Configuration conf = chart.getConfiguration();
    ... other chart configuration ...

    // In 3D!
    Options3d options3d = new Options3d();
    options3d.setEnabled(true);
    options3d.setAlpha(10);
    options3d.setBeta(30);
    options3d.setDepth(135); // Default is 100
    options3d.setViewDistance(100); // Default
    conf.getChart().setOptions3d(options3d);

The 3D options are as follows:

``

:   

`alpha`

:   The vertical tilt (pitch) in degrees.

`beta`

:   The horizontal tilt (yaw) in degrees.

`depth`

:   Depth of the third (Z) axis in pixel units.

`enabled`

:   Whether 3D plot is enabled. Default is `false`.

`frame`

:   Defines the 3D frame, which consists of a back, bottom, and side
    panels that display the chart grid.

        Frame frame = new Frame();
        frame.setBack(new FramePanel(SolidColor.BEIGE, 1));
        options3d.setFrame(frame);

`viewDistance`

:   View distance for creating perspective distortion. Default is 100.

### 3D Plot Options {#charts.basic-use.3d.plotoptions}

The above sets up the general 3D view, but you also need to configure
the 3D properties of the actual chart type. The 3D plot options are
chart type specific. For example, a pie has *depth* (or thickness),
which you can configure as follows:

    // Set some plot options
    PlotOptionsPie options = new PlotOptionsPie();
    ... Other plot options for the chart ...

    options.setDepth(45); // Our pie is quite thick

    conf.setPlotOptions(options);

### 3D Data {#charts.basic-use.3d.data}

For some chart types, such as pies and columns, the 3D view is merely a
visual representation for one- or two-dimensional data. Some chart
types, such as scatter charts, also feature a third, *depth axis*, for
data points. Such data points can be given as `DataSeriesItem3d`
objects.

The Z parameter is *depth* and is not scaled; there is no configuration
for the depth or Z axis. Therefore, you need to handle scaling yourself
as is done in the following.

    // Orthogonal data points in 2x2x2 cube
    double[][] points = { {0.0, 0.0, 0.0}, // x, y, z
                         {1.0, 0.0, 0.0},
                         {0.0, 1.0, 0.0},
                         {0.0, 0.0, 1.0},
                         {-1.0, 0.0, 0.0},
                         {0.0, -1.0, 0.0},
                         {0.0, 0.0, -1.0} };

    DataSeries series = new DataSeries();
    for (int i=0; i<points.length; i++) {
        double x = points[i][0];
        double y = points[i][1];
        double z = points[i][2];

        // Scale the depth coordinate, as the depth axis is
        // not scaled automatically
        DataSeriesItem3d item = new DataSeriesItem3d(x, y,
            z * options3d.getDepth().doubleValue());
        series.add(item);
    }
    conf.addSeries(series);

Above, we defined 7 orthogonal data points in the 2x2x2 cube centerd in
origo. The 3D depth was set to 135 earlier. The result is illustrated in
?.

![3D Scatter Chart](img/charts/charts-3d-scatter.png)

### Distance Fade {#charts.basic-use.3d.distance}

To add a bit more 3D effect, you can do some tricks, such as calculate
the distance of the data points from a viewpoint and set the marker size
and color according to the distance.

    public double distanceTo(double[] point, double alpha,
                             double beta, double viewDist) {
        final double theta = alpha * Math.PI / 180;
        final double phi   = beta * Math.PI / 180;
        double x = viewDist * Math.sin(theta) * Math.cos(phi);
        double y = viewDist * Math.sin(theta) * Math.sin(phi);
        double z = - viewDist * Math.cos(theta);
        return Math.sqrt(Math.pow(x - point[0], 2) +
                         Math.pow(y - point[1], 2) +
                         Math.pow(z - point[2], 2));
    }

Using the distance requires some assumptions about the scaling and such,
but for the data points (as defined earlier) in range -1.0 to +1.0 we
could do as follows:

    ...
    DataSeriesItem3d item = new DataSeriesItem3d(x, y,
        z * options3d.getDepth().doubleValue());

    double distance = distanceTo(new double[]{x,y,z},
                                 alpha, beta, 2); 

    int gr = (int) (distance*75); // Grayness
    Marker marker = new Marker(true);
    marker.setRadius(1 + 10 / distance);
    marker.setFillColor(new SolidColor(gr, gr, gr));
    item.setMarker(marker);

    series.add(item);

Note that here the view distance is in the scale of the data
coordinates, while the distance defined in the 3D options has different
definition and scaling. With the above settings, which are somewhat
exaggerated to illustrate the effect, the result is shown in ?.

![3D Distance Fade](img/charts/charts-3d-fade.png)

Chart Themes {#charts.basic-use.themes}
------------

The visual style and essentially any other chart configuration can be
defined in a *theme*. All charts shown in a UI may have only one theme,
which can be set with setTheme() in the `ChartOptions`.

The `ChartOptions` is a `UI` extension that is created and referenced by
calling the get() as follows:

    // Set Charts theme for the current UI
    ChartOptions.get().setTheme(new SkiesTheme());

The `VaadinTheme` is the default chart theme in Vaadin Charts. Other
available themes are `GrayTheme`, `GridTheme`, and `SkiesTheme`. The
default theme in Highcharts can be set with the
`HighChartsDefaultTheme`.

A theme is a Vaadin Charts configuration that is used as a template for
the configuration when rendering the chart.

Chart Types {#charts.charttypes}
===========

Vaadin Charts comes with over a dozen different chart types. You
normally specify the chart type in the constructor of the `Chart`
object. The available chart types are defined in the `ChartType` enum.
You can later read or set the chart type with the `chartType` property
of the chart model, which you can get with
getConfiguration().getChart().

Each chart type has its specific plot options and support its specific
collection of chart features. They also have specific requirements for
the data series.

The basic chart types and their variants are covered in the following
subsections.

Line and Spline Charts {#charts.charttypes.line}
----------------------

Line charts connect the series of data points with lines. In the basic
line charts the lines are straight, while in spline charts the lines are
smooth polynomial interpolations between the data points.

  ChartType                Plot Options Class
  ------------------------ ------------------------------------------------
  `LINE`                   `PlotOptionsLine`
  `SPLINE`                 `PlotOptionsSpline`

  : Line Chart Subtypes

### Plot Options {#charts.charttypes.line.plotoptions}

The `color` property in the line plot options defines the line color,
`lineWidth` the line width, and `dashStyle` the dash pattern for the
lines.

See ? for plot options regarding markers and other data point
properties. The markers can also be configured for each data point.

Area Charts {#charts.charttypes.area}
-----------

Area charts are like line charts, except that the area between the line
and the Y axis is painted with a transparent color. In addition to the
base type, chart type combinations for spline interpolation and ranges
are supported.

  ChartType                Plot Options Class
  ------------------------ ------------------------------------------------
  `AREA`                   `PlotOptionsArea`
  `AREASPLINE`             `PlotOptionsAreaSpline`
  `AREARANGE`              `PlotOptionsAreaRange`
  `AREASPLINERANGE`        `PlotOptionsAreaSplineRange`

  : Area Chart Subtypes

In area range charts, the area between a lower and upper value is
painted with a transparent color. The data series must specify the
minimum and maximum values for the Y coordinates, defined either with
`RangeSeries`, as described in ?, or with `DataSeries`, described in ?.

### Plot Options {#charts.charttypes.area.plotoptions}

Area charts support *stacking*, so that multiple series are piled on top
of each other. You enable stacking from the plot options with
setStacking(). The `Stacking.NORMAL` stacking mode does a normal
summative stacking, while the `Stacking.PERCENT` handles them as
proportions.

The fill color for the area is defined with the `fillColor` property and
its transparency with `fillOpacity` (the opposite of transparency) with
a value between 0.0 and 1.0.

The `color` property in the line plot options defines the line color,
`lineWidth` the line width, and `dashStyle` the dash pattern for the
lines.

See ? for plot options regarding markers and other data point
properties. The markers can also be configured for each data point.

Column and Bar Charts {#charts.charttypes.columnbar}
---------------------

Column and bar charts illustrate values as vertical or horizontal bars,
respectively. The two chart types are essentially equivalent, just as if
the orientation of the axes was inverted.

Multiple data series, that is, two-dimensional data, are shown with
thinner bars or columns grouped by their category, as described in ?.
Enabling stacking with setStacking() in plot options stacks the columns
or bars of different series on top of each other.

You can also have `COLUMNRANGE` charts that illustrate a range between a
lower and an upper value, as described in ?. They require the use of
`RangeSeries` for defining the lower and upper values.

  ChartType                Plot Options Class
  ------------------------ ------------------------------------------------
  `COLUMN`                 `PlotOptionsColumn`
  `COLUMNRANGE`            `PlotOptionsColumnRange`
  `BAR`                    `PlotOptionsBar`

  : Column and Bar Chart Subtypes

See the API documentation for details regarding the plot options.

Error Bars {#charts.charttypes.errorbar}
----------

An error bars visualize errors, or high and low values, in statistical
data. They typically represent high and low values in data or a
multitude of standard deviation, a percentile, or a quantile. The high
and low values are represented as horizontal lines, or "whiskers",
connected by a vertical stem.

While error bars technically are a chart type (`ChartType.ERRORBAR`),
you normally use them together with some primary chart type, such as a
scatter or column chart.

![Error Bars in a Scatter Chart](img/charts/charts-errorbar.png)

To display the error bars for data points, you need to have a separate
data series for the low and high values. The data series needs to use
the `PlotOptionsErrorBar` plot options type.

    // Create a chart of some primary type
    Chart chart = new Chart(ChartType.SCATTER);
    chart.setWidth("600px");
    chart.setHeight("400px");

    // Modify the default configuration a bit
    Configuration conf = chart.getConfiguration();
    conf.setTitle("Average Temperatures in Turku");
    conf.getLegend().setEnabled(false);

    // The primary data series
    ListSeries averages = new ListSeries(
        -6, -6.5, -4, 3, 9, 14, 17, 16, 11, 6, 2, -2.5);

    // Error bar data series with low and high values
    DataSeries errors = new DataSeries();
    errors.add(new DataSeriesItem(0,  -9, -3));
    errors.add(new DataSeriesItem(1, -10, -3));
    errors.add(new DataSeriesItem(2,  -8,  1));
    ...

    // Configure the stem and whiskers in error bars 
    PlotOptionsErrorBar barOptions = new PlotOptionsErrorBar();
    barOptions.setStemColor(SolidColor.GREY);
    barOptions.setStemWidth(2);
    barOptions.setStemDashStyle(DashStyle.DASH);
    barOptions.setWhiskerColor(SolidColor.BROWN);
    barOptions.setWhiskerLength(80); // 80% of category width
    barOptions.setWhiskerWidth(2); // Pixels
    errors.setPlotOptions(barOptions);

    // The errors should be drawn lower
    conf.addSeries(errors);
    conf.addSeries(averages);

Note that you should add the error bar series first, to have it rendered
lower in the chart.

### Plot Options {#charts.charttypes.errorbar.plotoptions}

Plot options for error bar charts have type `PlotOptionsErrorBar`. It
has the following chart-specific plot option properties:

`whiskerColor`, `whiskerWidth`, and `whiskerLength`

:   The color, width (vertical thickness), and length of the horizontal
    "whiskers" that indicate high and low values.

`stemColor`, `stemWidth`, and `stemDashStyle`

:   The color, width (thickness), and line style of the vertical "stems"
    that connect the whiskers. In box plot charts, which also have
    stems, they extend from the quadrintile box.

Box Plot Charts {#charts.charttypes.boxplot}
---------------

Box plot charts display the distribution of statistical variables. A
data point has a median, represented with a horizontal line, upper and
lower quartiles, represented by a box, and a low and high value,
represented with T-shaped "whiskers". The exact semantics of the box
symbols are up to you.

Box plot chart is closely related to the error bar chart described in ?,
sharing the box and whisker elements.

![Box Plot Chart](img/charts/charts-boxplot.png)

The chart type for box plot charts is `ChartType.BOXPLOT`. You normally
have just one data series, so it is meaningful to disable the legend.

    Chart chart = new Chart(ChartType.BOXPLOT);
    chart.setWidth("400px");
    chart.setHeight("300px");

    // Modify the default configuration a bit
    Configuration conf = chart.getConfiguration();
    conf.setTitle("Orienteering Split Times");
    conf.getLegend().setEnabled(false);

### Plot Options {#charts.charttypes.boxplot.plotoptions}

The plot options for box plots have type `PlotOptionsBoxPlot`, which
extends the slightly more generic `PlotOptionsErrorBar`. They have the
following plot option properties:

`medianColor`, `medianWidth`

:   Color and width (vertical thickness) of the horizontal median
    indicator line.

For example:

    // Set median line color and thickness
    PlotOptionsBoxPlot plotOptions = new PlotOptionsBoxPlot();
    plotOptions.setMedianColor(SolidColor.BLUE);
    plotOptions.setMedianWidth(3);
    conf.setPlotOptions(plotOptions);

### Data Model {#charts.charttypes.boxplot.datamodel}

As the data points in box plots have five different values instead of
the usual one, they require using a special `BoxPlotItem`. You can give
the different values with the setters, or all at once in the
constructor.

    // Orienteering control point times for runners
    double data[][] = orienteeringdata(); 

    DataSeries series = new DataSeries();
    for (double cpointtimes[]: data) {
        StatAnalysis analysis = new StatAnalysis(cpointtimes);
        series.add(new BoxPlotItem(analysis.low(),
                                   analysis.firstQuartile(),
                                   analysis.median(),
                                   analysis.thirdQuartile(),
                                   analysis.high()));
    }
    conf.setSeries(series);

If the "low" and "high" attributes represent an even smaller quantile,
or a larger multiple of standard deviation, you can have outliers. You
can plot them with a separate data series, with

Scatter Charts {#charts.charttypes.scatter}
--------------

Scatter charts display a set of unconnected data points. The name refers
to freely given X and Y coordinates, so the `DataSeries` or
`ContainerSeries` are usually the most meaningful data series types for
scatter charts.

![Scatter Chart](img/charts/charts-scatter.png)

The chart type of a scatter chart is `ChartType.SCATTER`. Its options
can be configured in a `PlotOptionsScatter` object, although it does not
have any chart-type specific options.

    Chart chart = new Chart(ChartType.SCATTER);
    chart.setWidth("500px");
    chart.setHeight("500px");
            
    // Modify the default configuration a bit
    Configuration conf = chart.getConfiguration();
    conf.setTitle("Random Sphere");
    conf.getLegend().setEnabled(false); // Disable legend

    PlotOptionsScatter options = new PlotOptionsScatter();
    // ... Give overall plot options here ...
    conf.setPlotOptions(options);
            
    DataSeries series = new DataSeries();
    for (int i=0; i<300; i++) {
        double lng = Math.random() * 2 * Math.PI;
        double lat = Math.random() * Math.PI - Math.PI/2;
        double x   = Math.cos(lat) * Math.sin(lng);
        double y   = Math.sin(lat);
        double z   = Math.cos(lng) * Math.cos(lat);
        
        DataSeriesItem point = new DataSeriesItem(x,y);
        Marker marker = new Marker();
        // Make settings as described later
        point.setMarker(marker);
        series.add(point);
    }
    conf.addSeries(series);

The result was shown in ?.

### Data Point Markers {#charts.charttypes.scatter.markers}

Scatter charts and other charts that display data points, such as line
and spline charts, visualize the points with *markers*. The markers can
be configured with the `Marker` property objects available from the plot
options of the relevant chart types, as well as at the level of each
data point, in the `DataSeriesItem`. You need to create the marker and
apply it with the setMarker() method in the plot options or the data
series item.

For example, to set the marker for an individual data point:

    DataSeriesItem point = new DataSeriesItem(x,y);
    Marker marker = new Marker();
    // ... Make any settings ...
    point.setMarker(marker);
    series.add(point);

### Marker Shape Properties {#charts.charttypes.scatter.markerproperties}

A marker has a `lineColor` and a `fillColor`, which are set using a
`Color` object. Both solid colors and gradients are supported. You can
use a `SolidColor` to specify a solid fill color by RGB values or choose
from a selection of predefined colors in the class.

    // Set line width and color
    marker.setLineWidth(1); // Normally zero width
    marker.setLineColor(SolidColor.BLACK);

    // Set RGB fill color
    int level = (int) Math.round((1-z)*127);
    marker.setFillColor(
            new SolidColor(255-level, 0, level));
    point.setMarker(marker);
    series.add(point);

You can also use a color gradient with `GradientColor`. Both linear and
radial gradients are supported, with multiple color stops.

Marker size is determined by the `radius` parameter, which is given in
pixels. The actual visual radius includes also the line width.

    marker.setRadius((z+1)*5);

### Marker Symbols {#charts.charttypes.scatter.markersymbols}

Markers are visualized either with a shape or an image symbol. You can
choose the shape from a number of built-in shapes defined in the
`MarkerSymbolEnum` enum (`CIRCLE`, `SQUARE`, `DIAMOND`, `TRIANGLE`, or
`TRIANGLE_DOWN`). These shapes are drawn with a line and fill, which you
can set as described above.

    marker.setSymbol(MarkerSymbolEnum.DIAMOND);

You can also use any image accessible by a URL by using a
`MarkerSymbolUrl` symbol. If the image is deployed with your
application, such as in a theme folder, you can determine its URL as
follows:

    String url = VaadinServlet.getCurrent().getServletContext()
        .getContextPath() + "/VAADIN/themes/mytheme/img/smiley.png";
    marker.setSymbol(new MarkerSymbolUrl(url));

The line, radius, and color properties are not applicable to image
symbols.

Bubble Charts {#charts.charttypes.bubble}
-------------

Bubble charts are a special type of scatter charts for representing
three-dimensional data points with different point sizes. We
demonstrated the same possibility with scatter charts in ?, but the
bubble charts make it easier to define the size of a point by its third
(Z) dimension, instead of the radius property. The bubble size is scaled
automatically, just like for other dimensions. The default point style
is also more bubbly.

![Bubble Chart](img/charts/charts-bubble.png)

The chart type of a bubble chart is `ChartType.BUBBLE`. Its options can
be configured in a `PlotOptionsBubble` object, which has a single
chart-specific property, `displayNegative`, which controls whether
bubbles with negative values are displayed at all. More typically, you
want to configure the bubble `marker`. The bubble tooltip is configured
in the basic configuration. The Z coordinate value is available in the
formatter JavaScript with `this.point.z` reference.

The bubble radius is scaled linearly between a minimum and maximum
radius. If you would rather scale by the area of the bubble, you can
approximate that by taking square root of the Z values.

In the following example, we overlay a bubble chart over a world map
background. We customize the bubbles to be more round with spherical
color gradient. Note that square root is taken of the Z coordinate to

    // Create a bubble chart 
    Chart chart = new Chart(ChartType.BUBBLE);
    chart.setWidth("640px"); chart.setHeight("350px");
            
    // Modify the default configuration a bit
    Configuration conf = chart.getConfiguration();
    conf.setTitle("Champagne Consumption by Country");
    conf.getLegend().setEnabled(false); // Disable legend
    conf.getTooltip().setFormatter("this.point.name + ': ' + " +
      "Math.round(100*(this.point.z * this.point.z))/100.0 + " +
      "' M bottles'");
            
    // World map as background
    String url = VaadinServlet.getCurrent().getServletContext()
        .getContextPath() + "/VAADIN/themes/mytheme/img/map.png";
    conf.getChart().setPlotBackgroundImage(url);

    // Show more bubbly bubbles with spherical color gradient
    PlotOptionsBubble plotOptions = new PlotOptionsBubble();
    Marker marker = new Marker();
    GradientColor color = GradientColor.createRadial(0.4, 0.3, 0.7);
    color.addColorStop(0.0, new SolidColor(255, 255, 255, 0.5));
    color.addColorStop(1.0, new SolidColor(170, 70, 67, 0.5));
    marker.setFillColor(color);
    plotOptions.setMarker(marker);
    conf.setPlotOptions(plotOptions);
            
    // Source: CIVC - Les expeditions de vins de Champagne en 2011
    DataSeries series = new DataSeries("Countries");
    Object data[][] = {
            {"France",         181.6},
            {"United Kingdom",  34.53},
            {"United States",   19.37},
            ...
    };
    for (Object[] country: data) {
        String name = (String) country[0];
        double amount = (Double) country[1];
        Coordinate pos = getCountryCoordinates(name); 

        DataSeriesItem3d item = new DataSeriesItem3d();
        item.setX(pos.longitude * Math.cos(pos.latitude/2.0 *
                                           (Math.PI/160)));
        item.setY(pos.latitude * 1.2);
        item.setZ(Math.sqrt(amount));
        item.setName(name);
        series.add(item);
    }
    conf.addSeries(series);
            
    // Set the category labels on the axis correspondingly
    XAxis xaxis = new XAxis();
    xaxis.setExtremes(-180, 180);
    ...
    conf.addxAxis(xaxis);

    // Set the Y axis title
    YAxis yaxis = new YAxis();
    yaxis.setExtremes(-90, 90);
    ...
    conf.addyAxis(yaxis);

Pie Charts {#charts.charttypes.pie}
----------

A pie chart illustrates data values as sectors of size proportionate to
the sum of all values. The pie chart is enabled with `ChartType.PIE` and
you can make type-specific settings in the `PlotOptionsPie` object as
described later.

    Chart chart = new Chart(ChartType.PIE);
    Configuration conf = chart.getConfiguration();
    ...

A ready pie chart is shown in ?.

![Pie Chart](img/charts/charts-pie.png)

### Plot Options {#charts.charttypes.pie.plotoptions}

The chart-specific options of a pie chart are configured with a
`PlotOptionsPie`.

    PlotOptionsPie options = new PlotOptionsPie();
    options.setInnerSize(0); // Non-0 results in a donut
    options.setSize("75%");  // Default
    options.setCenter("50%", "50%"); // Default
    conf.setPlotOptions(options);

`innerSize`
:   A pie with inner size greater than zero is a "donut". The inner size
    can be expressed either as number of pixels or as a relative
    percentage of the chart area with a string (such as "60%") See the
    section later on donuts.

`size`
:   The size of the pie can be expressed either as number of pixels or
    as a relative percentage of the chart area with a string (such as
    "80%"). The default size is 75%, to leave space for the labels.

`center`
:   The X and Y coordinates of the center of the pie can be expressed
    either as numbers of pixels or as a relative percentage of the chart
    sizes with a string. The default is "50%", "50%".

### Data Model {#charts.charttypes.pie.data}

The labels for the pie sectors are determined from the labels of the
data points. The `DataSeries` or `ContainerSeries`, which allow labeling
the data points, should be used for pie charts.

    DataSeries series = new DataSeries();
    series.add(new DataSeriesItem("Mercury", 4900));
    series.add(new DataSeriesItem("Venus", 12100));
    ...
    conf.addSeries(series);

If a data point, as defined as a `DataSeriesItem` in a `DataSeries`, has
the *sliced* property enabled, it is shown as slightly cut away from the
pie.

    // Slice one sector out
    DataSeriesItem earth = new DataSeriesItem("Earth", 12800);
    earth.setSliced(true);
    series.add(earth);

### Donut Charts {#charts.charttypes.pie.donut}

Setting the `innerSize` of the plot options of a pie chart to a larger
than zero value results in an empty hole at the center of the pie.

    PlotOptionsPie options = new PlotOptionsPie();
    options.setInnerSize("60%");
    conf.setPlotOptions(options);

As you can set the plot options also for each data series, you can put
two pie charts on top of each other, with a smaller one fitted in the
"hole" of the donut. This way, you can make pie charts with more details
on the outer rim, as done in the example below:

    // The inner pie
    DataSeries innerSeries = new DataSeries();
    innerSeries.setName("Browsers");
    PlotOptionsPie innerOptions = new PlotOptionsPie();
    innerPieOptions.setSize("60%");
    innerSeries.setPlotOptions(innerPieOptions);
    ...

    DataSeries outerSeries = new DataSeries();
    outerSeries.setName("Versions");
    PlotOptionsPie outerOptions = new PlotOptionsPie();
    outerOptions.setInnerSize("60%");
    outerSeries.setPlotOptions(outerSeriesOptions);
    ...

The result is illustrated in ?.

![Overlaid Pie and Donut Chart](img/charts/charts-donut.png)

Gauges {#charts.charttypes.gauge}
------

A gauge is an one-dimensional chart with a circular Y-axis, where a
rotating pointer points to a value on the axis. A gauge can, in fact,
have multiple Y-axes to display multiple scales.

A *solid gauge* is otherwise like a regular gauge, except that a solid
color arc is used to indicate current value instead of a pointer. The
color of the indicator arc can be configured to change according to
color stops.

Let us consider the following gauge:

    Chart chart = new Chart(ChartType.GAUGE);
    chart.setWidth("400px");
    chart.setHeight("400px");

After the settings done in the subsequent sections, it will show as in
?.

![A Gauge](img/charts/charts-gauge.png)

### Gauge Configuration {#charts.charttypes.gauge.conf}

The start and end angles of the gauge can be configured in the `Pane`
object of the chart configuration. The angles can be given as -360 to
360 degrees, with 0 at the top of the circle.

    Configuration conf = chart.getConfiguration();
    conf.setTitle("Speedometer");
    conf.getPane().setStartAngle(-135);
    conf.getPane().setEndAngle(135);

### Axis Configuration {#charts.charttypes.gauge.axis}

A gauge has only an Y-axis. You need to provide both a minimum and
maximum value for it.

    YAxis yaxis = new YAxis();
    yaxis.setTitle("km/h");

    // The limits are mandatory
    yaxis.setMin(0);
    yaxis.setMax(100);

    // Other configuration
    yaxis.getLabels().setStep(1);
    yaxis.setTickInterval(10);
    yaxis.setPlotBands(new PlotBand[]{
            new PlotBand(0,  60,  SolidColor.GREEN),
            new PlotBand(60, 80,  SolidColor.YELLOW),
            new PlotBand(80, 100, SolidColor.RED)});

    conf.addyAxis(yaxis);

You can do all kinds of other configuration to the axis - please see the
API documentation for all the available parameters.

### Setting and Updating Gauge Data {#charts.charttypes.gauge.data}

A gauge only displays a single value, which you can define as a data
series of length one, such as as follows:

    ListSeries series = new ListSeries("Speed", 80);
    conf.addSeries(series);

Gauges are especially meaningful for displaying changing values. You can
use the updatePoint() method in the data series to update the single
value.

    final TextField tf = new TextField("Enter a new value");
    layout.addComponent(tf);

    Button update = new Button("Update", new ClickListener() {
        @Override
        public void buttonClick(ClickEvent event) {
            Integer newValue = new Integer((String)tf.getValue());
            series.updatePoint(0, newValue);
        }
    }); 
    layout.addComponent(update);

Solid Gauges {#charts.charttypes.solidgauge}
------------

A solid gauge is much like a regular gauge described previously; a
one-dimensional chart with a circular Y-axis. However, instead of a
rotating pointer, the value is indicated by a rotating arc with solid
color. The color of the indicator arc can be configured to change
according to the value using color stops.

Let us consider the following gauge:

    Chart chart = new Chart(ChartType.SOLIDGAUGE);
    chart.setWidth("400px");
    chart.setHeight("400px");

After the settings done in the subsequent sections, it will show as in
?.

![A Solid Gauge](img/charts/charts-solidgauge.png)

While solid gauge is much like a regular gauge, the configuration
differs

### Configuration {#charts.charttypes.solidgauge.conf}

The solid gauge must be configured in the drawing `Pane` of the chart
configuration. The gauge arc spans an angle, which is specified as -360
to 360 degrees, with 0 degrees at the top of the arc. Typically, a
semi-arc is used, where you use -90 and 90 for the angles, and move the
center lower than you would have with a full circle. You can also adjust
the size of the gauge pane; enlargening it allows positioning tick
labels better.

    Configuration conf = chart.getConfiguration();
    conf.setTitle("Solid Gauge");

    Pane pane = conf.getPane();
    pane.setSize("125%");           // For positioning tick labels
    pane.setCenterXY("50%", "70%"); // Move center lower
    pane.setStartAngle(-90);        // Make semi-circle
    pane.setEndAngle(90);           // Make semi-circle

The shape of the gauge display is defined as the background of the pane.
You at least need to set the shape as either "`arc`" or "`solid`". You
typically also want to set background color and inner and outer radius.

    Background bkg = new Background();
    bkg.setBackgroundColor(new SolidColor("#eeeeee")); // Gray
    bkg.setInnerRadius("60%");  // To make it an arc and not circle
    bkg.setOuterRadius("100%"); // Default - not necessary
    bkg.setShape("arc");        // solid or arc
    pane.setBackground(bkg);

### Axis Configuration {#charts.charttypes.solidgauge.axis}

A gauge only has an Y-axis. You must define the value range (*min* and
*max*).

    YAxis yaxis = new YAxis();
    yaxis.setTitle("Pressure GPa");
    yaxis.getTitle().setY(-70); // Move 70 px upwards from center

    // The limits are mandatory
    yaxis.setMin(0);
    yaxis.setMax(200);

    // Configure ticks and labels
    yaxis.setTickInterval(100);  // At 0, 100, and 200
    yaxis.getLabels().setY(-16); // Move 16 px upwards

You can configure color stops for the indicator arc. The stops are
defined with `Stop` objects having stop points from 0.0 to 1.0 and color
values.

    yaxis.setStops(new Stop(0.1f, SolidColor.GREEN),
                   new Stop(0.5f, SolidColor.YELLOW),
                   new Stop(0.9f, SolidColor.RED));

    conf.addyAxis(yaxis);

Setting yaxis.getLabels().setRotationPerpendicular() makes gauge labels
rotate perpendicular to the center.

You can do all kinds of other configuration to the axis - please see the
API documentation for all the available parameters.

### Plot Options {#charts.charttypes.solidgauge.plotoptions}

Solid gauges do not currently have any chart type specific plot options.
See ? for common options.

    PlotOptionsSolidGauge options = new PlotOptionsSolidGauge();

    // Move the value display box at the center a bit higher
    Labels dataLabels = new Labels();
    dataLabels.setY(-20);
    options.setDataLabels(dataLabels);

    conf.setPlotOptions(options);

### Setting and Updating Gauge Data {#charts.charttypes.solidgauge.data}

A gauge only displays a single value, which you can define as a data
series of length one, such as as follows:

    ListSeries series = new ListSeries("Pressure MPa", 80);
    conf.addSeries(series);

Gauges are especially meaningful for displaying changing values. You can
use the updatePoint() method in the data series to update the single
value.

    final TextField tf = new TextField("Enter a new value");
    layout.addComponent(tf);

    Button update = new Button("Update", new ClickListener() {
        @Override
        public void buttonClick(ClickEvent event) {
            Integer newValue = new Integer((String)tf.getValue());
            series.updatePoint(0, newValue);
        }
    }); 
    layout.addComponent(update);

Area and Column Range Charts {#charts.charttypes.rangecharts}
----------------------------

Ranged charts display an area or column between a minimum and maximum
value, instead of a singular data point. They require the use of
`RangeSeries`, as described in ?. An area range is created with
`AREARANGE` chart type, and a column range with `COLUMNRANGE` chart
type.

Consider the following example:

    Chart chart = new Chart(ChartType.AREARANGE);
    chart.setWidth("400px");
    chart.setHeight("300px");

    // Modify the default configuration a bit
    Configuration conf = chart.getConfiguration();
    conf.setTitle("Extreme Temperature Range in Finland");
    ...

    // Create the range series
    // Source: http://ilmatieteenlaitos.fi/lampotilaennatyksia
    RangeSeries series = new RangeSeries("Temperature Extremes",
        new Double[]{-51.5,10.9},
        new Double[]{-49.0,11.8},
        ...
        new Double[]{-47.0,10.8});//
    conf.addSeries(series);

The resulting chart, as well as the same chart with a column range, is
shown in ?.

![Area and Column Range Chart](img/charts/charts-arearange.png)

Polar, Wind Rose, and Spiderweb Charts {#charts.charttypes.polar}
--------------------------------------

Most chart types having two axes can be displayed in *polar*
coordinates, where the X axis is curved on a circle and Y axis from the
center of the circle to its rim. Polar chart is not a chart type in
itself, but can be enabled for most chart types with setPolar(true) in
the chart model parameters. Therefore all chart type specific features
are usable with polar charts.

Vaadin Charts allows many sorts of typical polar chart types, such as
*wind rose*, a polar column graph, or *spiderweb*, a polar chart with
categorical data and a more polygonal visual style.

    // Create a chart of some type
    Chart char = new Chart(ChartType.LINE);

    // Enable the polar projection
    Configuration conf = chart.getConfiguration();
    conf.getChart().setPolar(true);

You need to define the sector of the polar projection with a `Pane`
object in the configuration. The sector is defined as degrees from the
north direction. You also need to define the value range for the X axis
with setMin() and setMax().

    // Define the sector of the polar projection
    Pane pane = new Pane(0, 360); // Full circle
    conf.addPane(pane);

    // Define the X axis and set its value range
    XAxis axis = new XAxis();
    axis.setMin(0);
    axis.setMax(360);

The polar and spiderweb charts are illustrated in ?.

![Wind Rose and Spiderweb Charts](img/charts/charts-polarspiderweb.png)

### Spiderweb Charts {#charts.charttypes.polar.spiderweb}

A *spiderweb* chart is a commonly used visual style of a polar chart
with a polygonal shape rather than a circle. The data and the X axis
should be categorical to make the polygonal interpolation meaningful.
The sector is assumed to be full circle, so no angles for the pane need
to be specified. Note the style settings done in the axis in the example
below:

    Chart chart = new Chart(ChartType.LINE);
    ...

    // Modify the default configuration a bit
    Configuration conf = chart.getConfiguration();
    conf.getChart().setPolar(true);
    ...

    // Create the range series
    // Source: http://ilmatieteenlaitos.fi/lampotilaennatyksia
    ListSeries series = new ListSeries("Temperature Extremes",
        10.9, 11.8, 17.5, 25.5, 31.0, 33.8,
        37.2, 33.8, 28.8, 19.4, 14.1, 10.8);
    conf.addSeries(series);

    // Set the category labels on the X axis correspondingly
    XAxis xaxis = new XAxis();
    xaxis.setCategories("Jan", "Feb", "Mar",
        "Apr", "May", "Jun", "Jul", "Aug", "Sep",
        "Oct", "Nov", "Dec");
    xaxis.setTickmarkPlacement(TickmarkPlacement.ON);
    xaxis.setLineWidth(0);
    conf.addxAxis(xaxis);

    // Configure the Y axis
    YAxis yaxis = new YAxis();
    yaxis.setGridLineInterpolation("polygon"); // Webby look
    yaxis.setMin(0);
    yaxis.setTickInterval(10);
    yaxis.getLabels().setStep(1);
    conf.addyAxis(yaxis);

Funnel and Pyramid Charts {#charts.charttypes.funnel}
-------------------------

Funnel and pyramid charts are typically used to visualize stages in a
sales processes, and for other purposes to visualize subsets of
diminishing size. A funnel or pyramid chart has layers much like a
stacked column: in funnel from top-to-bottom and in pyramid from
bottom-to-top. The top of the funnel has width of the drawing area of
the chart, and dinimishes in size down to a funnel "neck" that continues
as a column to the bottom. A pyramid diminishes from bottom to top and
does not have a neck.

![Funnel and Pyramid Charts](img/charts/charts-funnel.png)

Funnels have chart type `FUNNEL`, pyramids have `PYRAMID`.

The labels of the funnel blocks are by default placed on the right side
of the blocks, together with a connector. You can configure their style
in the plot options, as is done in the following example.

    Chart chart = new Chart(ChartType.FUNNEL);
    chart.setWidth("500px");
    chart.setHeight("350px");
            
    // Modify the default configuration a bit
    Configuration conf = chart.getConfiguration();
    conf.setTitle("Monster Utilization");
    conf.getLegend().setEnabled(false);
            
    // Give more room for the labels
    conf.getChart().setSpacingRight(120);

    // Configure the funnel neck shape 
    PlotOptionsFunnel options = new PlotOptionsFunnel();
    options.setNeckHeightPercentage(20);
    options.setNeckWidthPercentage(20);

    // Style the data labels
    Labels dataLabels = new Labels();
    dataLabels.setFormat("<b>{point.name}</b> ({point.y:,.0f})");
    dataLabels.setSoftConnector(false);
    dataLabels.setColor(SolidColor.BLACK);
    options.setDataLabels(dataLabels);
          
    conf.setPlotOptions(options);

    // Create the range series
    DataSeries series = new DataSeries();
    series.add(new DataSeriesItem("Monsters Met", 340));
    series.add(new DataSeriesItem("Engaged", 235));
    series.add(new DataSeriesItem("Killed", 187));
    series.add(new DataSeriesItem("Tinned", 70));
    series.add(new DataSeriesItem("Eaten", 55));
    conf.addSeries(series);

### Plot Options {#charts.charttypes.funnel.plotoptions}

The funnel and pyramid options are configured with `PlotOptionsFunnel`
or `PlotOptionsFunnel`, respectively.

In addition to common chart options, the chart types support the
following shared options: `width`, `height`, `depth`,
`allowPointSelect`, `borderColor`, `borderWidth`, `center`,
`slicedOffset`, and `visible`. See ? for detailed descriptions.

They have the following chart type specific properties:

`neckHeight` or `neckHeightPercentage` (only funnel)
:   Height of the neck part of the funnel either as pixels or as
    percentage of the entire funnel height.

`neckWidth` or `neckWidthPercentage` (only funnel)
:   Width of the neck part of the funnel either as pixels or as
    percentage of the top of the funnel.

`reversed`
:   Whether the chart is reversed upside down from the normal direction
    from diminishing from the top to bottom. The default is
    false
    for funnel and
    true
    for pyramid.

Waterfall Charts {#charts.charttypes.waterfall}
----------------

Waterfall charts are used for visualizing level changes from an initial
level to a final level through a number of changes in the level. The
changes are given as delta values, and you can have a number of
intermediate totals, which are calculated automatically.

![Waterfall Charts](img/charts/charts-waterfall.png)

Waterfall charts have chart type `WATERFALL`. For example:

    Chart chart = new Chart(ChartType.WATERFALL);
    chart.setWidth("500px");
    chart.setHeight("350px");

    // Modify the default configuration a bit
    Configuration conf = chart.getConfiguration();
    conf.setTitle("Changes in Reindeer Population in 2011");
    conf.getLegend().setEnabled(false);

    // Configure X axis
    XAxis xaxis = new XAxis();
    xaxis.setCategories("Start", "Predators", "Slaughter",
        "Reproduction", "End");
    conf.addxAxis(xaxis);

    // Configure Y axis
    YAxis yaxis = new YAxis();
    yaxis.setTitle("Population (thousands)");
    conf.addyAxis(yaxis);
    ...

The example continues in the following subsections.

### Plot Options {#charts.charttypes.waterfall.plotoptions}

Waterfall charts have plot options type `PlotOptionsWaterfall`, which
extends the more general options defined in `PlotOptionsColumn`. It has
the following chart type specific properties:

`upColor`
:   Color for the positive values. For negative values, the
    negativeColor
    defined in
    PlotOptionsColumn
    is used.

In the following, we define the colors, as well as the style and
placement of the labels for the columns:

    // Define the colors
    final Color balanceColor = SolidColor.BLACK;
    final Color positiveColor = SolidColor.BLUE;
    final Color negativeColor = SolidColor.RED;

    // Configure the colors 
    PlotOptionsWaterfall options = new PlotOptionsWaterfall();
    options.setUpColor(positiveColor);
    options.setNegativeColor(negativeColor);

    // Configure the labels
    Labels labels = new Labels(true);
    labels.setVerticalAlign(VerticalAlign.TOP);
    labels.setY(-20);
    labels.setFormatter("Math.floor(this.y/1000) + 'k'");
    Style style = new Style();
    style.setColor(SolidColor.BLACK);
    style.setFontWeight(FontWeight.BOLD);
    labels.setStyle(style);
    options.setDataLabels(labels);
    options.setPointPadding(0);
    conf.setPlotOptions(options);

### Data Series {#charts.charttypes.waterfall.datamodel}

The data series for waterfall charts consists of changes (deltas)
starting from an initial value and one or more cumulative sums. There
should be at least a final sum, and optionally intermediate sums. The
sums are represented as `WaterFallSum` data items, and no value is
needed for them as they are calculated automatically. For intermediate
sums, you should set the `intermediate` property to `true`.

    // The data
    DataSeries series = new DataSeries();

    // The beginning balance
    DataSeriesItem start = new DataSeriesItem("Start", 306503);
    start.setColor(balanceColor);
    series.add(start);

    // Deltas
    series.add(new DataSeriesItem("Predators", -3330));
    series.add(new DataSeriesItem("Slaughter", -103332));
    series.add(new DataSeriesItem("Reproduction", +104052));

    WaterFallSum end = new WaterFallSum("End");
    end.setColor(balanceColor);
    end.setIntermediate(false); // Not intermediate (default)
    series.add(end);

    conf.addSeries(series);

Heat Maps {#charts.charttypes.heatmap}
---------

A heat map is a two-dimensional grid, where the color of a grid cell
indicates a value.

![Heat Maps](img/charts/charts-heatmap.png)

Heat maps have chart type `HEATMAP`. For example:

    Chart chart = new Chart(ChartType.HEATMAP);
    chart.setWidth("600px");
    chart.setHeight("300px");

    Configuration conf = chart.getConfiguration();
    conf.setTitle("Heat Data");

    // Set colors for the extremes
    conf.getColorAxis().setMinColor(SolidColor.AQUA);
    conf.getColorAxis().setMaxColor(SolidColor.RED);

    // Set up border and data labels
    PlotOptionsHeatMap plotOptions = new PlotOptionsHeatMap();
    plotOptions.setBorderColor(SolidColor.WHITE);
    plotOptions.setBorderWidth(2);
    plotOptions.setDataLabels(new Labels(true));
    conf.setPlotOptions(plotOptions);

    // Create some data
    HeatSeries series = new HeatSeries();
    series.addHeatPoint( 0, 0,  10.9); // Jan High
    series.addHeatPoint( 0, 1, -51.5); // Jan Low
    series.addHeatPoint( 1, 0,  11.8); // Feb High
    ...
    series.addHeatPoint(11, 1, -47.0); // Dec Low
    conf.addSeries(series);

    // Set the category labels on the X axis
    XAxis xaxis = new XAxis();
    xaxis.setTitle("Month");
    xaxis.setCategories("Jan", "Feb", "Mar",
        "Apr", "May", "Jun", "Jul", "Aug", "Sep",
        "Oct", "Nov", "Dec");
    conf.addxAxis(xaxis);

    // Set the category labels on the Y axis
    YAxis yaxis = new YAxis();
    yaxis.setTitle("");
    yaxis.setCategories("High C", "Low C");
    conf.addyAxis(yaxis);

### Heat Map Data Series {#charts.charttypes.heatmap.dataseries}

Heat maps require two-dimensional tabular data. The easiest way is to
use `HeatSeries`, as was done in the example earlier. You can add data
points with the addHeatPoint() method, or give all the data at once in
an array with setData() or in the constructor.

If you need to use other data series type for a heat map, notice that
the semantics of the heat map data points are currently not supported by
the general-purpose series types, such as `DataSeries`. You can work
around this semantic limitation by specifying the X (column), Y (row),
and heatScore by using the respective X, low, and high properties of the
general-purpose data series.

Also note that while some other data series types allow updating the
values one by one, updating all the values in a heat map is very
inefficient; it is faster to simply replace the data series and then
call chart.drawChart().

Chart Configuration {#charts.configuration}
===================

All the chart content configuration of charts is defined in a *chart
model* in a `Configuration` object. You can access the model with the
getConfiguration() method.

The configuration properties in the `Configuration` class are summarized
in the following:

-   credits: `Credits` (text, position, href, enabled)

-   labels: `HTMLLabels` (html, style)

-   lang: `Lang` (decimalPoint, thousandsSep, loading)

-   legend: `Legend` (see ?)

-   pane: `Pane`

-   plotoptions: `PlotOptions` (see ?

-   series: Series

-   subTitle: `SubTitle`

-   title: `Title`

-   tooltip: `Tooltip`

-   xAxis: `XAxis` (see ?

-   yAxis: `YAxis` (see ?

For data configuration, see ?.

Plot Options {#charts.configuration.plotoptions}
------------

The plot options can be set in the configuration of the entire chart or
for each data series separately. Some of the plot options are chart type
specific, defined in type-specific options classes, which all extend
`AbstractPlotOptions`.

You need to create the plot options object and set them either for the
entire chart or for a data series with setPlotOptions().

For example, the following enables stacking in column charts:

    PlotOptionsColumn plotOptions = new PlotOptionsColumn();
    plotOptions.setStacking(Stacking.NORMAL);
    conf.setPlotOptions(plotOptions);

See the API documentation of each chart type and its plot options class
for more information about the chart-specific options, and the
`AbstractPlotOptions` for the shared plot options.

### Other Options {#charts.configuration.plotoptions.other}

The following options are supported by some chart types.

`width` and `widthPercentage`
:   Defines the width of the chart either by pixels or as a percentual
    proportion of the drawing area.

`height` and `heightPercentage`
:   Defines the height of the chart either by pixels or as a percentual
    proportion of the drawing area.

`depth`
:   Specifies the thickness of the chart in 3D mode.

`allowPointSelect`
:   Specifies whether data points, in whatever way they are visualized
    in the particular chart type, can be selected by clicking on them.
    Defaults to
    false
    .

`borderColor`
:   Defines the border color of the chart elements.

`borderWidth`
:   Defines the width of the border in pixels.

`center`
:   Defines the center of the chart within the chart area by left and
    top coordinates, which can be specified either as pixels or as a
    percentage (as string) of the drawing area. The default is top 50%
    and left 50%.

`slicedOffset`
:   In chart types that support slices, such as pie and pyramid charts,
    specifies the offset for how far a slice is detached from other
    items. The amount is given in pixels and defaults to 10 pixels.

`visible`
:   Specifies whether or not a chart is visible. Defaults to
    true
    .

Axes {#charts.configuration.axes}
----

Different chart types may have one, two, or three axes; in addition to X
and Y axes, some chart types may have a color axis. These are
represented by `XAxis`, `YAxis`, and `ColorAxis`, respectively. The X
axis is usually horizontal, representing the iteration over the data
series, and Y vertical, representing the values in the data series. Some
chart types invert the axes and they can be explicitly inverted with
getChart().setInverted() in the chart configuration. An axis has a
caption and tick marks at intervals indicating either numeric values or
symbolic categories. Some chart types, such as gauge, have only Y-axis,
which is circular in the gauge, and some such as a pie chart have none.

The basic elements of X and Y axes are illustrated in ?.

![Chart Axis Elements](img/charts/charts-axes-hi.png)

Axis objects are created and added to the configuration object with
addxAxis() and addyAxis().

    XAxis xaxis = new XAxis();
    xaxis.setTitle("Axis title");
    conf.addxAxis(xaxis);

A chart can have more than one Y-axis, usually when different series
displayed in a graph have different units or scales. The association of
a data series with an axis is done in the data series object with
setyAxis().

For a complete reference of the many configuration parameters for the
axes, please refer to the JavaDoc API documentation of Vaadin Charts.

### Axis Type {#charts.configuration.axes.type}

Axes can be one of the following types, which you can set with
setType(). The axis types are enumerated under `AxisType`. `LINEAR` is
the default.

`LINEAR` (default)
:   For numeric values in linear scale.

`LOGARITHMIC`
:   For numerical values, as in the linear axis, but the axis will be
    scaled in the logarithmic scale. The minimum for the axis
    must
    be a positive non-zero value (
    log(0)
    is not defined, as it has limit at negative infinity when the
    parameter approaches zero).

`DATETIME`
:   Enables date/time mode in the axis. The date/time values are
    expected to be given either as a
    Date
    object or in milliseconds since the Java (or Unix) date epoch on
    January 1st 1970 at 00:00:00 GMT. You can get the millisecond
    representation of Java
    Date
    with
    getTime()
    .

`CATEGORY`
:   Enables using categorical data for the axis, as described in more
    detail later. With this axis type, the category labels are
    determined from the labels of the data points in the data series,
    without need to set them explicitly with
    setCategories()
    .

### Categories {#charts.configuration.axes.categories}

The axes display, in most chart types, tick marks and labels at some
numeric interval by default. If the items in a data series have a
symbolic meaning rather than numeric, you can associate *categories*
with the data items. The category label is displayed between two axis
tick marks and aligned with the data point. In certain charts, such as
column chart, where the corresponding values in different data series
are grouped under the same category. You can set the category labels
with setCategories(), which takes the categories as (an ellipsis)
parameter list, or as an iterable. The list should match the items in
the data series.

    XAxis xaxis = new XAxis();
    xaxis.setCategories("Mercury", "Venus", "Earth",
                        "Mars", "Jupiter", "Saturn",
                        "Uranus", "Neptune");

You can only set the category labels from the data point labels by
setting the axis type to `CATEGORY`, as described earlier.

### Labels {#charts.configuration.axes.labels}

The axes display, in most chart types, tick marks and labels at some
numeric interval by default. The format and style of labels in an axis
is defined in a `Labels` object, which you can get with getLabels() from
the axis.

    XAxis xaxis = new XAxis();
    ...
    Labels xlabels = xaxis.getLabels();
    xlabels.setAlign(HorizontalAlign.CENTER); // Default
    xlabels.setBackgroundColor(SolidColor.PALEGREEN);
    xlabels.setBorderWidth(3);
    xlabels.setColor(SolidColor.GREEN);
    xlabels.setRotation(-45);
    xlabels.setStep(2); // Every 2 major tick

Axis labels have the following configuration properties:

`align`
:   Defines the alignment of the labels relative to the centers of the
    ticks. On left alignment, the left edges of labels are aligned at
    the tickmarks, and correspondingly the right side on right
    alignment. The default is determined automatically based on the
    direction of the axis and rotation of the labels.

`distance` (only in polar charts)
:   Distance of labels from the perimeter of the plot area, in pixels.

`enabled`
:   Whether labels are enabled or not. Defaults to
    true
    .

`format`
:   Formatting string for labels, as described in
    . Defaults to "
    {value}
    ".

`formatter`

:   A JavaScript formatter for the labels, as described in ?. The value
    is available in the `this.value` property. The `this` object also
    has `axis`, `chart`, `isFirst`, and `isLast` properties. Defaults
    to:

        function() {return this.value;}

`maxStaggerLines` (only horizontal axis)
:   When labels on the horizontal (usually X) axis are displayed so
    densely that they would overlap, they are automatically placed on
    alternating lines in "staggered" fashion. When number of lines is
    not set manually with
    staggerLines
    , this parameter defines the maximum number of such lines; value 1
    disables automatic staggering. Default is 5 lines.

`rotation`

:   Defines rotation of labels in degrees. A positive value indicates
    rotation in clockwise direction. Labels are rotated at their
    alignment point. Defaults to 0.

        Labels xlabels = xaxis.getLabels();
        xlabels.setAlign(HorizontalAlign.RIGHT);
        xlabels.setRotation(-45); // Tilt 45 degrees CCW

`staggerLines`
:   Defines number of lines for placing the labels to avoid overlapping.
    By default undefined, and the number of lines is automatically
    determined up to
    maxStaggerLines
    .

`step`

:   Defines tick interval for showing labels, so that labels are shown
    at every *n*th tick. The default step is automatically determined,
    along with staggering, to avoid overlap.

        Labels xlabels = xaxis.getLabels();
        xlabels.setStep(2); // Every 2 major tick

`style`

:   Defines style for labels. The property is a `Style` object, which
    has to be created and set.

        Labels xlabels = xaxis.getLabels();
        Style xlabelsstyle = new Style();
        xlabelsstyle.setColor(SolidColor.GREEN);
        xlabels.setStyle(xlabelsstyle);

`useHTML`
:   Allows using HTML in custom label formats. Otherwise, HTML is
    quoted. Defaults to
    false
    .

`x`, `y`
:   Offsets for the label's position, relative to the tick position. X
    offset defaults to 0, but Y to
    null
    , which enables automatic positioning based on font size.

Gauge, pie, and polar charts allow additional properties.

For a complete reference of the many configuration parameters for the
labels, please refer to the JavaDoc API documentation of Vaadin Charts.

### Axis Range {#charts.configuration.axes.extremes}

The axis range is normally set automatically to fit the data, but can
also be set explicitly. The *extremes* property in the axis
configuration defines the minimum and maximum values of the axis range.
You can set them either individually with setMin() and setMax(), or
together with setExtremes(). Changing the extremes programmatically
requires redrawing the chart with drawChart().

Legend {#charts.configuration.legend}
------

The legend is a box that describes the data series shown in the chart.
It is enabled by default and is automatically populated with the names
of the data series as defined in the series objects, and the
corresponding color symbol of the series.

Formatting Labels {#charts.configuration.format}
-----------------

Data point values, tooltips, and tick labels are formatted according to
formatting configuration for the elements, with configuration properties
described earlier for each element. Formatting can be set up in the
overall configuration, for a data series, or for individual data points.
The format can be defined either by a format string or by JavaScript
formatter, which are described in the following.

### Using Format Strings {#charts.configuration.format.string}

A formatting string contain free-form text mixed with variables.
Variables are enclosed in brackets, such as "`Here
                    {point.y} is a value at {point.x}`". In different
contexts, you have at least the following variables available:

-   value
    in axis labels
-   point.x
    ,
    point.x
    in data points and tooltips
-   series.name
    in data points and tooltips
-   series.color
    in data points and tooltips

Values can be formatted according to a formatting string, separated from
the variable name by a colon.

For numeric values, a subset of C printf formatting specifiers is
supported. For example, "`{point.y:%02.2f}` would display a
floating-point value with two decimals and two leading zeroes, such as
`02.30`.

For dates, you can use a subset of PHP strftime() formatting specifiers.
For example, "`{value:%Y-%m-%d %H:%M:%S}`" would format a date and time
in the ISO 8601 format.

### Using a JavaScript Formatter {#charts.configuration.format.formatter}

A JavaScript formatter is given in a string that defines a JavaScript
function that returns the formatted string. The value to be formatted is
available in `this.value` for axis labels, or `this.x`, `this.y` for
data points.

For example, to format tick labels on a chart axis, you could have:

    YAxis yaxis = new YAxis();
    Labels ylabels = yaxis.getLabels();
     ylabels.setFormatter("function() {return this.value + ' km';}");

### Simplified Formatting {#charts.configuration.format.simplified}

Some contexts that display labels allow defining simple formatting for
the labels. For example, data point tooltips allow defining prefix,
suffix, and floating-point precision for the values.

Chart Data {#charts.data}
==========

Chart data is stored in data series model, which contains visual
representation information about the data points in addition to their
values. There are a number of different types of series - `DataSeries`,
`ListSeries`, `AreaListSeries`, and `RangeSeries`.

List Series {#charts.data.listseries}
-----------

The `ListSeries` is essentially a helper type that makes the handling of
simple sequential data easier than with `DataSeries`. The data points
are assumed to be at a constant interval on the X axis, starting from
the value specified with the `pointStart` property (default is 0) at
intervals specified with the `pointInterval` property (default is 1.0).
The two properties are defined in the `PlotOptions` for the series.

The Y axis values are given in a `List<Number>`, or with ellipsis or an
array.

    ListSeries series = new ListSeries(
          "Total Reindeer Population",
          181091, 201485, 188105, ...);
    series.getPlotOptions().setPointStart(1959);
    conf.addSeries(series);

You can also add them one by one with the addData() method, which is
typical when converting from some other representation.

    // Original representation
    int data[][] = reindeerData();

    // Create a list series with X values starting from 1959
    ListSeries series = new ListSeries("Reindeer Population");
    series.getPlotOptions().setPointStart(1959);

    // Add the data points
    for (int row[]: data)
        series.addData(data[1]);

    conf.addSeries(series);

If the chart has multiple Y axes, you can specify the axis for the
series by its index number with setyAxis().

Generic Data Series {#charts.data.dataseries}
-------------------

The `DataSeries` can represent a sequence of data points at an interval
as well as scatter data. Data points are represented with the
`DataSeriesItem` class, which has `x` and `y` properties for
representing the data value. Each item can be given a category name.

    DataSeries series = new DataSeries();
    series.setName("Total Reindeer Population");
    series.add(new DataSeriesItem(1959, 181091));
    series.add(new DataSeriesItem(1960, 201485));
    series.add(new DataSeriesItem(1961, 188105));
    series.add(new DataSeriesItem(1962, 177206));

    // Modify the color of one point
    series.get(1960, 201485)
        .getMarker().setFillColor(SolidColor.RED);
    conf.addSeries(series);

Data points are associated with some visual representation parameters:
marker style, selected state, legend index, and dial style (for gauges).
Most of them can be configured at the level of individual data series
items, the series, or in the overall plot options for the chart. The
configuration options are described in ?. Some parameters, such as the
sliced option for pie charts is only meaningful to configure at item
level.

### Adding and Removing Data Items {#charts.data.dataseries.add}

New `DataSeriesItem` items are added to a series with the add() method.
The basic method takes just the data item, but the other method takes
also two boolean parameters. If the `updateChart` parameter is `false`,
the chart is not updated immediately. This is useful if you are adding
many points in the same request.

The `shift` parameter, when `true`, causes removal of the first data
point in the series in an optimized manner, thereby allowing an animated
chart that moves to left as new points are added. This is most
meaningful with data with even intervals.

You can remove data points with the remove() method in the series.
Removal is generally not animated, unless a data point is added in the
same change, as is caused by the `shift` parameter for the add().

### Updating Data Items {#charts.data.dataseries.update}

If you update the properties of a `DataSeriesItem` object, you need to
call update() method for the series with the item as the parameter.
Changing the coordinates of a data point in this way causes animation of
the change.

### Range Data {#charts.data.dataseries.range}

Range charts expect the Y values to be specified as minimum-maximum
value pairs. The `DataSeriesItem` provides setLow() and setHigh()
methods to set the minimum and maximum values of a data point, as well
as a number of constructors that accept the values.

    RangeSeries series =
        new RangeSeries("Temperature Extremes");

    // Give low-high values in constructor
    series2.add(new DataSeriesItem(0, -51.5, 10.9));
    series2.add(new DataSeriesItem(1, -49.0, 11.8));

    // Set low-high values with setters
    DataSeriesItem point2 = new DataSeriesItem();
    point2.setX(2);
    point2.setLow(-44.3);
    point2.setHigh(17.5);
    series2.add(point2);

The `RangeSeries` offers a slightly simplified way of adding ranged data
points, as described in ?.

Range Series {#charts.data.rangeseries}
------------

The `RangeSeries` is a helper class that extends `DataSeries` to allow
specifying interval data a bit easier, with a list of minimum-maximum
value ranges in the Y axis. You can use the series in range charts, as
described in ?.

For X axis, the coordinates are generated at fixed intervals starting
from the value specified with the `pointStart` property (default is 0)
at intervals specified with the `pointInterval` property (default is
1.0).

### Setting the Data {#charts.data.rangeseries.data}

The data in a `RangeSeries` is given as an array of minimum-maximum
value pairs for the Y value axis. The pairs are also represented as
arrays. You can pass the data using the ellipsis in the constructor or
the setData():

    RangeSeries series =
        new RangeSeries("Temperature Ranges",
        new Double[]{-51.5,10.9},
        new Double[]{-49.0,11.8},
        ...
        new Double[]{-47.0,10.8});
    conf.addSeries(series);

Or, as always with variable arguments, you can also pass them in an
array, in the following for the setData():

    series.setData(new Double[][] {
        new Double[]{-51.5,10.9},
        new Double[]{-49.0,11.8},
        ...
        new Double[]{-47.0,10.8}});

Container Data Series {#charts.data.containerseries}
---------------------

The `ContainerDataSeries` is an adapter for binding Vaadin Container
data sources to charts. The container needs to have properties that
define the name, X-value, and Y-value of a data point. The default
property IDs of the three properties are "`name`", "`x`", and "`y`",
respectively. You can set the property IDs with setNamePropertyId(),
setYPropertyId(), and setXPropertyId(), respectively. If the container
has no `x` property, the data is assumed to be categorical.

In the following example, we have a `BeanItemContainer` with `Planet`
items, which have a `name` and `diameter` property. We display the
container data both in a Vaadin `Table` and a chart.

    // The data
    BeanItemContainer<Planet> container =
            new BeanItemContainer<Planet>(Planet.class);
    container.addBean(new Planet("Mercury", 4900));
    container.addBean(new Planet("Venus", 12100));
    container.addBean(new Planet("Earth", 12800));
    ...

    // Display it in a table
    Table table = new Table("Planets", container);
    table.setPageLength(container.size());
    table.setVisibleColumns("name","diameter");
    layout.addComponent(table);

    // Display it in a chart
    Chart chart = new Chart(ChartType.COLUMN);
    ... Configure it ...

    // Wrap the container in a data series
    ContainerDataSeries series =
            new ContainerDataSeries(container);

    // Set up the name and Y properties
    series.setNamePropertyId("name");
    series.setYPropertyId("diameter");

    conf.addSeries(series);

As the X axis holds categories rather than numeric values, we need to
set up the category labels with an array of string. There are a few ways
to do that, some more efficient than others, below is one way:

    // Set the category labels on the axis correspondingly
    XAxis xaxis = new XAxis();
    String names[] = new String[container.size()];
    List<Planet> planets = container.getItemIds();
    for (int i=0; i<planets.size(); i++)
        names[i] = planets.get(i).getName();
    xaxis.setCategories(names);
    xaxis.setTitle("Planet");
    conf.addxAxis(xaxis);

The result can be seen in ?.

![Table and Chart Bound to a
Container](img/charts/charts-containerdataseries.png)

Advanced Uses {#charts.advanced}
=============

Server-Side Rendering and Exporting {#charts.advanced.export}
-----------------------------------

In addition to using charts in Vaadin UIs, you may also need to provide
them as images or in downloadable documents. Vaadin Charts can be
rendered on the server-side using a headless JavaScript execution
environment, such as [PhantomJS](#).

Vaadin Charts supports a HighCharts remote export service, but the SVG
Generator based on PhantomJS is almost as easy to use and allows much
more powerful uses.

### Using a Remote Export Service {#charts.advanced.export.exporting}

HighCharts has a simple built-in export functionality that does the
export in a remote export server. HighCharts provides a default export
service, but you can also configure your own.

You can enable the built-in export function by setting
setExporting(true) in the chart configuration.

    chart.getConfiguration().setExporting(true);

To configure it further, you can provide a `Exporting` object with
custom settings.

    // Create the export configuration
    Exporting exporting = new Exporting(true);
            
    // Customize the file name of the download file
    exporting.setFilename("mychartfile.pdf");

    // Enable export of raster images 
    exporting.setEnableImages(true);

    // Use the exporting configuration in the chart
    chart.getConfiguration().setExporting(exporting);

If you only want to enable download, you can disable the print button as
follows:

    ExportButton printButton = new ExportButton();
    printButton.setEnabled(false);
    exporting.setPrintButton(printButton);

The functionality uses a HighCharts export service by default. To use
your own, you need to set up one and then configure it in the exporting
configuration as follows:

    exporting.setUrl("http://my.own.server.com");

### Using the SVG Generator {#charts.advanced.export.svggenerator}

The `SVGGenerator` in Vaadin Charts provides an advanced way to render
the Chart into SVG format on the server-side. SVG is well supported by
many applications, can be converted to virtually any other graphics
format, and can be passed to PDF report generators.

The generator uses PhantomJS to render the chart on the server-side. You
need to install it from [phantomjs.org](#). After installation,
PhantomJS should be in your system path. If not, you can set the
`phantom.exec` system property for the JRE to point to the PhantomJS
binary.

To generate the SVG image content as a string (it's XML), simply call
the generate() method in the `SVGGenerator` singleton and pass it the
chart configuration.

    String svg = SVGGenerator.getInstance()
        .generate(chart.getConfiguration());

You can then use the SVG image as you like, for example, for download
from a `StreamResource`, or include it in a HTML, PDF, or other
document. You can use SVG tools such as the [Batik](#) or [iText](#)
libraries to generate documents. For a complete example, you can check
out the Charts Export Demo from the Subversion repository at
<https://github.com/vaadin/charts/tree/master/chart-export-demo>.

Timeline {#charts.timeline}
========

The `Timeline` is a charting component in the Vaadin Charts add-on
separate from the `Chart` component. Its purpose is to give the user an
intuitive understanding of events and trends on a horizontal timeline
axis.

`Timeline` uses its own representation for the data series, different
from the `Chart` and more optimized for updating. You can represent
almost any time-related statistical data that has a time-value mapping.
Multiple data sources can be used to allow comparison between data.

![Timeline Component](img/addons/timeline-overview.png)

A timeline allows representing time-related data visually as graphs
instead of numerical values. They are used commonly in almost all fields
of business, science, and technology, such as in project management to
map out milestones and goals, in geology to map out historical events,
and perhaps most prominently in the stock market.

With Timeline, you can represent almost any time-related statistical
data that has a time-value mapping. Even several data sources can be
used for comparison between data. This allows the user to better grasp
of changes in the data and antipate forthcoming trends and problems.

Graph types {#charts.timeline.graphtypes}
-----------

The Vaadin Timeline supports three graph types:

*Line graphs*
:   Useful for representing continuous data, such as temperature changes
    or changes in stock price.

*Bar graphs*
:   Useful for representing discrete or discontinuous data, such as
    market share or forum posts.

*Scatter graphs*
:   Useful for representing discrete or discontinuous data.

If you have several graphs in the timeline, you can also stack them on
top of each other instead of drawing them on top of each other by
setting setGraphStacking() in `Timeline` to `true`.

Interaction Elements {#charts.timeline.interaction}
--------------------

The user can interact with the Vaadin Timeline in several ways.

On the bottom of the timeline there is a *scrollbar area* where you can
move the time forward or backward in time by dragging the time range box
or by clicking the left and right arrow buttons. You can change the time
range by resizing the range box in the scrollbar area. You can also zoom
with the mouse wheel when the pointer is inside the component.

![Scrollbar Area](img/addons/timeline-interaction-scrollarea.png)

The middle area of the timeline is the *main area* where the selected
time range is displayed. Time scale is shown below the main area. The
time scale used depends on the zoom level and can be a time unit from
hours to years. Value scale is displayed on the right side of the main
area. The scale can be either a static value range or a range calculated
from the displayed data set. The user can move in time by dragging the
main area with the mouse left and right and zoom in and out by using the
mouse wheel.

![Main Area](img/addons/timeline-interaction-mainarea.png)

You can select a *preset zoom level* with the buttons on the top the
Timeline. This will change the displayed time range to match the zoom
level. The zoom levels are fully customizable to suit the time range in
the API.

![Preset Zoom Buttons](img/addons/timeline-interaction-presetzooms.png)

The *current time range* is shown at the top-right corner of the
component. Clicking the dates makes them editable, so that you can
manually change them. *Graph legend* is shown below the time range. The
legend explains what is represented by each bar on the graph and
displays the current value when the user moves the mouse cursor over the
graph.

![Current Time Range and Graph
Legend](img/addons/timeline-interaction-timerange.png)

Finally, the available *chart modes* are shown below the preset zoom
levels options. The available graph modes can be set from the API.

![Chart Mode](img/addons/timeline-interaction-chartmode.png)

You can use or hide any of the features above can be shown or hidden
depending on your needs. For example, if you only need to display a
graph without any controls, you can hide all them from the API.

Event Markers {#charts.timeline.events}
-------------

In addition to graphs, the timeline can have events. An event can be,
for example, the time of a published advertisement in a graph that
displays website hits. Combining the event data with the graphs enables
the user to observe the relevance of the advertisement to the website
hits visually.

Vaadin Timeline provides two types of event markers, as illustrated in
?.

![Timeline Event Markers](img/addons/timeline-event-markers.png)

(On left) Marker with a customizable marker sign, for example, letter
'E'. The marker displays a caption which appears when the user hovers
the pointer over the event.

(On right) Marker with button-like appearance with a marker sign and a
caption.

Efficiency {#charts.timeline.efficiency}
----------

Vaadin Timeline reduces the traffic between the server and the client by
using two methods. First, all the data that is presented in the
component is dynamically fetched from the server as needed. This means
that when the user scrolls the timeline view, the component continuously
fetches data from the server. Also, only data that is visible to the
user is transferred to the client. For example, if the timeline has data
that has been measured once a second for an entire year, not all the
data will be sent to the client. Only the data which can be rendered on
the screen without overlapping is sent. This ensures that, even for
large data sets, the loading time is small and only the necessary data
is actually transferred over the network.

Second, Vaadin Timeline caches the data received from the server in the
browser, so that the data is transferred over the network only once, if
possible. This speeds up the time-range browsing when data can be
fetched from the cache instead of reloading it over the network.

Data Source Requirements {#charts.timeline.data-source}
------------------------

Vaadin Timeline uses Vaadin containers as data sources for both the
graphs and the events. There are, however, some requirements for the
containers to make them compatible with the Vaadin Timeline.

The containers have to implement Container.Indexed for the Vaadin
Timeline to be able to use them. This is because the Vaadin Timeline
dynamically fetches the data from the server when needed. This way large
data sets can be used without having to load all data to the client-side
at once and it brings a huge performance increase.

Another requirement is that the container has one property of type
`java.util.Date` (or a class that can be cast to it), which contains the
timestamp when a data point or event occurred. This property has to be
set by using the setGraphTimestampPropertyId() in `Timeline`. The
default property ID `timeline.PropertyId.TIMESTAMP` is used if no
timestamp-property ID has been set.

A graph container also needs to have a *value* property that defines the
value of the data point. This value can be any numerical value. The
value property can be set with setGraphValuePropertyId() in `Timeline`.
The default property ID `Timeline.PropertyId.VALUE` is used if no value
property is given.

Below is an example of how a graph container could be constructed:

    // Construct a container which implements Container.Indexed       
    IndexedContainer container = new IndexedContainer();

    // Add the Timestamp property to the container
    Object timestampProperty = "Our timestamp property";
    container.addContainerProperty(timestampProperty,
                                   java.util.Date.class, null);

    // Add the value property
    Object valueProperty = "Our value property";
    container.addContainerProperty(valueProperty, Float.class, null);

    // Our timeline
    Timeline timeline = new Timeline();

    // Add the container as a graph container
    timeline.addGraphDataSource(container, timestampProperty,
                                           valueProperty);

The event and marker containers are similar. They both need the
`timestamp` property which should be of type `java.util.Date` and the
`caption` property which should be a string. The marker container
additionally needs a `value` property which is displayed in the marker
popup.

Below is an example on how a marker or event container can be
constructed:

    // Create the container
    IndexedContainer container = new IndexedContainer();
            
    // Add the timestamp property
    container.addContainerProperty(Timeline.PropertyId.TIMESTAMP,
                                   Date.class, null);
            
    // Add the caption property
    container.addContainerProperty(Timeline.PropertyId.CAPTION,
                                  String.class, "");

    // Add the marker specific value property.
    // Not needed for a event containers.
    container.addContainerProperty(Timeline.PropertyId.VALUE,
                                   String.class, "");

    // Create the timeline with the container as both the marker
    // and event data source
    Timeline timeline = new Timeline();
    timeline.setMarkerDataSource(container, 
        Timeline.PropertyId.TIMESTAMP,
        Timeline.PropertyId.CAPTION,
        Timeline.PropertyId.VALUE);

    timeline.setEventDataSource(container,
        Timeline.PropertyId.TIMESTAMP,
        Timeline.PropertyId.CAPTION);

The above example uses the default property IDs. You can change them to
suit your needs.

The `Timeline` listens for changes in the containers and updates the
graph accordingly. When it updates the graph and items are added or
removed from the container, the currently selected date range will
remain selected. The selection bar in the browser area moves to keep the
current selection selected. If you want the selection to change when the
contents of the container changes and keep the selection area
stationary, you can disable the selection lock by setting
setBrowserSelectionLock() to `false`.

Events and Listeners {#charts.timeline.events}
--------------------

Two types of events are available when using the Vaadin Timeline.

### Date Range Changes {#charts.timeline.events.daterange}

When the user modifies the selected date range by moving the date range
selector, dragging the timeline, or by manually entering new dates, an
event will be sent to the server with the information of what the
current displayed date range is. To listen to these events you can
attach a `DateRangeListener` which will receive the start and end dates
of the current selection.

### Event Clicks {#charts.timeline.events.eventclick}

If the timeline has events, you can add an `EventClickListener` to
listen for clicks on the events. The listener will receive a list of
item IDs which are related to the click event from the event data
source. Multiple events can be combined into a single event icon if
space is not sufficient for displaying them all, in which case many item
IDs can be returned.

Configurability {#charts.timeline.configurability}
---------------

The Vaadin Timeline is highly customizable and its outlook can be easily
changed to suit your needs. The default view of the Timeline contains
all the controls available but often all of them are not needed and can
be hidden.

The following list contains the components that can be shown or hidden
at your preference:

-   Chart modes
-   Textual date select
-   Browser area (bottom part of the Timeline)
-   Legend
-   Zoom levels
-   Caption

The outlook of the graphs themselves can also be changed for both the
browser area and the main view. The following settings are available
through the API:

-   Graph outline color
-   Graph outline width
-   Graph caps (in line graphs only)
-   Graph fill color
-   Graph visibility
-   Graph shadows

Other changes to the outlook of the component can easily be done by CSS.

Zoom levels are also fully customizable. Zoom levels are defined as
milliseconds and can be added by calling the addZoomLevel() method. A
zoom level always has a caption, which is the visible part in the zoom
panel, and a millisecond amount.

By default the grid divides the graph into five equally spaced parts
with a gray color. However, you can fully customize how the grid is
drawn by using setGridColor() and setVerticalGridLines().

Localization {#charts.timeline.localization}
------------

By default the Vaadin Timeline uses English as its primary language for
the captions and the default locale for the application to display the
dates in the timeline.

You can change the different captions in the Timeline by using their
corresponding setters:

-   setZoomLevelsCaption()
    -- The caption appearing before the zoom levels
-   setChartModesCaption()
    -- The caption appearing before the chart modes

Furthermore, you can also change the locale in which the Timeline shows
the dates in the horizontal scale by specifying a valid locale using the
setLocale() method of the timeline.

You can also configure in what format the dates appear in the horizontal
scale or in the date select in the top-right corner by using the
getDateFormats()-method which will return a `DateFormatInfo` object. By
using its setters you can set specific formats for each date range in
the scale. Please note that if you are using long date formats they
might get clipped if the scale does not fit the whole formatted date.

Timeline Tutorial {#charts.timeline.code-example}
-----------------

In the following tutorial, we look step-by-step how to create a
timeline.

### Create the Data Sources {#charts.timeline.code-example.data-sources}

To use the Timeline, you need to create some data sources for it.
Timeline uses Container.Indexed containers as data sources for both the
graphs and the markers and events. So lets start by creating a
datasource which represents the graph we want to draw in the timeline.

For the Timeline to understand how the data is constructed in the
container we need to use specific property ids which describe what kind
of data each property represents. For the Vaadin Timeline to work
properly we will need to add two property ids, one for when the value
was acquired and one for the value itself. The Vaadin Timeline has these
both properties predefined as `Timeline.PropertyId.TIMESTAMP` and
`Timeline.PropertyId.VALUE`. You can use the predefined ones or create
your own if you wish.

So, lets create a container which meets the above stated specification.
Open the main UI class which was automatically created when we created
the project and add the following method.

    /**
     * Creates a graph container with a month of random data
     */
    public Container.Indexed createGraphDataSource(){
            
        // Create the container
        Container.Indexed container = new IndexedContainer();
            
        // Add the required property ids (use the default ones here)
        container.addContainerProperty(Timeline.PropertyId.TIMESTAMP, 
            Date.class, null);
        container.addContainerProperty(Timeline.PropertyId.VALUE, 
            Float.class, 0f);
            
        // Add some random data to the container
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.MONTH, -1);
        Date today = new Date();
        Random generator = new Random();
            
        while(cal.getTime().before(today)){
            // Create  a point in time
            Item item = container.addItem(cal.getTime());
                
            // Set the timestamp property
            item.getItemProperty(Timeline.PropertyId.TIMESTAMP)
                .setValue(cal.getTime());
                
            // Set the value property
            item.getItemProperty(Timeline.PropertyId.VALUE)
                .setValue(generator.nextFloat());
            
            cal.add(Calendar.DAY_OF_MONTH, 1);            
        }
            
        return container;        
    }

This method will create an indexed container with some random points. As
you can see we are using an `IndexedContainer` and define two properties
to it which was discussed earlier. Then we just generate some random
data in the container. Here we are using the default property ids for
the timestamp and value but you could use your own if you wished. We'll
see later how you would tell the Timeline which property ids to use if
you used your own.

Next, lets add some markers to our graph. Markers are arrow like shapes
in the bottom of the timeline with which you can mark some occurrence
that happened at that time. To create markers you again have to create a
data source for them. I'll first show you how the code to create them
and then explain what it all means. Add the following method to the UI
class:

    /**
     * Creates a marker container with a marker for each seven days
     */
    public Container.Indexed createMarkerDataSource(){
            
        // Create the container
        Container.Indexed container = new IndexedContainer();
            
        // Add the required property IDs (use the default ones here)
        container.addContainerProperty(Timeline.PropertyId.TIMESTAMP,
                Date.class, null);
        container.addContainerProperty(Timeline.PropertyId.CAPTION, 
                String.class, "Our marker symbol");
        container.addContainerProperty(Timeline.PropertyId.VALUE, 
                String.class, "Our description");
            
        // Add a marker for every seven days
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.MONTH, -1);
        Date today = new Date();
        SimpleDateFormat formatter =
                new SimpleDateFormat("EEE, MMM d, ''yy");
        while(cal.getTime().before(today)){
            // Create a point in time
            Item item = container.addItem(cal.getTime());
            
            // Set the timestamp property
            item.getItemProperty(Timeline.PropertyId.TIMESTAMP)
                    .setValue(cal.getTime());
            
            // Set the caption property
            item.getItemProperty(Timeline.PropertyId.CAPTION)
                    .setValue("M");
                
            // Set the value property
            item.getItemProperty(Timeline.PropertyId.VALUE).
               setValue("Today is "+formatter.format(cal.getTime()));

            cal.add(Calendar.DAY_OF_MONTH, 7);
        }
        
        return container;        
    }

Here we start the same as in the example with the graph container by
creating an indexed container. Remember, all containers must be indexed
containers when using the graph component.

We then add the timestamp property, caption property and value property.

The timestamp property is the same as in the graph container but the
caption and value property differ. The caption property describes what
kind of marker it is. The caption is displayed on top of the arrow shape
in the Timeline so it should be a short symbol, preferably only one
character long. The class of the caption property must be String.

The value property should also be a string and is displayed when the
user hovers the mouse over the marker. This string can be arbitrarily
long and normally should represent some kind of description of the
marker.

The third kind of data sources are the event data sources. The events
are displayed on top of the timeline and supports grouping and are
clickable. They are represented as button like icons in the Timeline.

The event data sources are almost identical the to marker data sources
except the value property is missing. Lets create an event data source
and add events for each Sunday in out graph:

    /**
     * Creates a event container with a marker for each sunday
     */
    public Container.Indexed createEventDataSource(){

        // Create the container
        Container.Indexed container = new IndexedContainer();
        
        // Add the required property IDs (use default ones here)
        container.addContainerProperty(
                Timeline.PropertyId.TIMESTAMP, Date.class, null);
        container.addContainerProperty(
                Timeline.PropertyId.CAPTION,
                String.class, "Our marker symbol");
                
        // Add a marker for every seven days
        Calendar cal = Calendar.getInstance();
        cal.add(Calendar.MONTH, -1);
        Date today = new Date();        
        while(cal.getTime().before(today)){
            if (cal.get(Calendar.DAY_OF_WEEK) == Calendar.SUNDAY){
                // Create a point in time
                Item item = container.addItem(cal.getTime());

                // Set the timestamp property
                item.getItemProperty(Timeline.PropertyId.TIMESTAMP)
                    .setValue(cal.getTime());
                    
                // Set the caption property
                item.getItemProperty(Timeline.PropertyId.CAPTION)
                    .setValue("Sunday");            
            }           
            cal.add(Calendar.DAY_OF_MONTH, 1);
        }
            
        return container;       
    }

As you can see the event container does not differ a whole lot from the
marker containers. In use however they differ since they are groupable
they can be closely put together and still be usable and you can add
click listeners to them so you can catch user events. More on the click
listeners later.

So now we have our three data sources ready to be displayed in our
application. In the next chapter we will use them with our Timeline and
see how they integrate with it.

### Create the Timeline {#charts.timeline.code-example.timeline}

Okay, now that we have out data sources lets look at the init-method in
our Vaadin Application. Lets start by creating our timeline, so add the
following line to the end of the init-method in
`MytimelinedemoApplication`:

    Timeline timeline = new Timeline("Our timeline");
    timeline.setWidth("100%");

This will create the timeline we want with a 100 percent width. Now lets
add our data sources to the timeline:

    timeline.addGraphDataSource(createGraphDataSource(), 
                            Timeline.PropertyId.TIMESTAMP,
                            Timeline.PropertyId.VALUE);

    timeline.setMarkerDataSource(createMarkerDataSource(), 
                            Timeline.PropertyId.TIMESTAMP, 
                            Timeline.PropertyId.CAPTION,
                            Timeline.PropertyId.VALUE);

    timeline.setEventDataSource(createEventDataSource(), 
                            Timeline.PropertyId.TIMESTAMP,
                            Timeline.PropertyId.CAPTION);

And finally add the timeline to the UI. Here is the complete
init-method:

    @Override
    protected void init(VaadinRequest request) {
        VerticalLayout content = new VerticalLayout();
        setContent(content);
            
        // Create the timeline
        Timeline timeline = new Timeline("Our timeline");

        // Create the data sources
        Container.Indexed graphDS  = createGraphDataSource();
        Container.Indexed markerDS = createMarkerDataSource();
        Container.Indexed eventDS  = createEventDataSource();
            
        // Add our data sources
        timeline.addGraphDataSource(graphDS, 
                                    Timeline.PropertyId.TIMESTAMP,
                                    Timeline.PropertyId.VALUE);
        timeline.setMarkerDataSource(markerDS, 
                                     Timeline.PropertyId.TIMESTAMP,
                                     Timeline.PropertyId.CAPTION,     
                                     Timeline.PropertyId.VALUE);
        timeline.setEventDataSource(eventDS, 
                                    Timeline.PropertyId.TIMESTAMP,
                                    Timeline.PropertyId.CAPTION);
            
        content.addComponent(timeline);        
    }

Now you should be able to start the application and browse the timeline.
The result is shown in ?.

![Timeline Example
Application](img/timeline/timeline-example-timeline.png)

### Final Touches {#charts.timeline.code-example.final}

Now that we have our timeline we would probably like to customize it a
bit. There are many things you can do but lets start by giving our graph
some style properties and a caption in the legend. This can be done as
follows:

    // Set the caption of the graph
    timeline.setGraphLegend(graphDataSource, "Our cool graph");
            
    // Set the color of the graph
    timeline.setGraphOutlineColor(graphDataSource, Color.RED);

    // Set the fill color of the graph
    timeline.setGraphFillColor(graphDataSource, new Color(255,0,0,128));
            
    // Set the width of the graph
    timeline.setGraphOutlineThickness(2.0);

Lets do the same to the browser areas graph:

    // Set the color of the browser graph
    timeline.setBrowserOutlineColor(graphDataSource, Color.BLACK);

    // Set the fill color of the graph
    timeline.setBrowserFillColor(graphDataSource,
                                 new Color(0,0,0,128));

And the result looks like this:

![Styling Timeline](img/timeline/timeline-example-result.png)

Okay, now that looks different. But there is still something missing. If
you look in the upper left corner you will not see any zoom levels. No
zoom levels are predefined so we will have to make our own. Since we are
dealing with a month of data lets make a zoom level for a day, a week
and a month. Zoom levels are given in milliseconds so we will have to
calculate how many milliseconds each of the zoom levels are. So lets add
them by adding the following lines:

    // Add some zoom levels
    timeline.addZoomLevel("Day", 86400000L);
    timeline.addZoomLevel("Week", 7 * 86400000L);
    timeline.addZoomLevel("Month", 2629743830L);

Remember the events we added? You can now see them in the graph but
their functionality is still a bit incomplete. We can add an event
listener to the graph which will send an event each time the user clicks
on one of the event buttons. To demonstrate this feature lets add an
event listener which notifies the user what date the Sunday-button
represents. Here is the code for that:

    // Listen to click events from events
    timeline.addListener(new Timeline.EventClickListener() {
        @Override
        public void eventClick(EventButtonClickEvent event) {
            Item item = eventDataSource.getItem(event.getItemIds()
                                       .iterator().next());
            Date sunday = (Date) item.getItemProperty(
                          Timeline.PropertyId.TIMESTAMP).getValue();
            SimpleDateFormat formatter =
                new SimpleDateFormat("EEE, MMM d, ''yy");
            
            Notification.show(formatter.format(sunday));
        }        
    });

Now try clicking on the events and see what happens!

And here is the final demo application, yours will probably look a bit
different since we are using random data.

![Final Example](img/timeline/timeline-example-final.png)

Now we hope you have a basic understanding of how the Timeline works and
how it can be customized. There are still a few features we left out of
this tutorial like hiding unnecessary components from the timeline and
adding multiple graphs to the timeline, but these are pretty self
explanatory features and you probably can look them up in the JavaDoc.
