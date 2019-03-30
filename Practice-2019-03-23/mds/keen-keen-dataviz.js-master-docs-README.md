# Keen.Dataviz API

## Chart types

Powered by [C3.js](http://c3js.org/examples.html):

* [area](http://c3js.org/samples/chart_area.html)
* [area-spline](http://c3js.org/samples/chart_area.html)
* [area-step](http://c3js.org/samples/chart_step.html)
* [bar](http://c3js.org/samples/chart_bar.html)
* [donut](http://c3js.org/samples/chart_donut.html)
* [gauge](http://c3js.org/samples/chart_gauge.html)
* [line](http://c3js.org/samples/simple_multiple.html)
* [pie](http://c3js.org/samples/chart_pie.html)
* [spline](http://c3js.org/samples/chart_spline.html)
* [step](http://c3js.org/samples/chart_step.html)

Types like "bar" and "line" support [axis rotation](http://c3js.org/samples/axes_rotated.html). To make this easier to achieve, an additional series of types have been exposed, which automatically handle this configuration:

* horizontal-area
* horizontal-area-spline
* horizontal-area-step
* horizontal-bar
* horizontal-line
* horizontal-spline
* horizontal-step

### Custom (built by us):

* metric

If `previousResults` is set, the metric will show the difference between the current result and the previous one.

```javascript
const chart = new KeenDataviz({
    container: '#some_container',
    type: 'metric',
    results,
    previousResults // optional
  })
```

* message

* funnel

* horizontal-funnel

Default configuration values:

```javascript
  funnel: {
    lines: true, // separate each step with a line
    resultValues: true, // show or hide results
    percents: {
      show: false, // show and hide percents
      countingMethod: 'absolute', // 'absolute' - use the value of the first step to calculate the percentage change
                                  // 'relative' - use the value of the previous step to calculate the percentage change
      decimals: 0, // the number of decimal digits visible
    }
    hover: true, // show or hide hover effect
    marginBetweenSteps: false, // show or hide spaces between elements
  }
```

3D funnels have similar options, with a small change:

* funnel-3d

```javascript
  funnel: {
    marginBetweenSteps: false, // N/A
    effect3d: 'both-sides' // 'both-sides' - showing shades on both sides
                           // 'left' - showing shades on left side
                           // 'right' - showing shades on right side
  }
```

* horizontal-funnel-3d

```javascript
  funnel: {
    marginBetweenSteps: false, // N/A
    effect3d: 'both-sides' // 'both-sides' - showing shades on top and bottom
                           // 'top' - showing shades on top
                           // 'right' - showing shades on bottom
  }
```

## Prototype methods

### .call(function)

Call arbitrary functions within the prototype/instance context.

```javascript
chart
  .call(function(){
    const total = this.data().slice(1).length;
    this.title('Total Results: ' + total);
  })
```

### .data()

This method accepts two forms of input data.

* **Object:** Raw data, either from a query API response or a manually constructed object
* **Keen.Dataset instance**

**Important:** objects passed into `.data()` will be inspected to infer which type of response is being handled, and to parse accordingly. This is where the library determines a default `type` (listed above) to use, if no type value has been set.

```javascript
// Object
chart.data({ result: 621 });

// Dataset instance
const ds = new Keen.Dataset();
ds.set(['Value', 'Result'], 621);
chart.data(ds);

// Return current Dataset.data() output
chart.data();

// Check out what 'type' we have set:
chart.type(); // 'metric' for this example
```

### .destroy()

Destroy a visualization and remove all rendered DOM elements.

```javascript
chart.destroy();
```

### .library(string)

Specify the library for a visualization. _Default value is default_.

```javascript
chart.library('my-custom-library');
```

### .message(string)

Display a message for a visualization. _Previously `.error()`)_.

```javascript
chart.message('Oops, an error occured!');
```

