spacetimeline
=============

SpaceTimeLine A conceptual FRP model library


https://github.com/kenokabe/spacetimeline

##Install

`npm install spacetimeline`

##CDN
 http://kentutorialbook.github.io/cdn/spacetimeline.js

##Live Demo Webpage
 [facebook/react](https://github.com/facebook/react)+spacetimeline live-demo web-page.

http://kentutorialbook.github.io/demo/frp-redball-delay/index.html

https://github.com/kentutorialbook/kentutorialbook.github.io/tree/master/demo/frp-redball-delay

In this live-demo, the FRP library records all mouse-move event with a time-stamp.

Or **all mouse-move event in 10 seconds duration**. You can limit the time-line data size by setting `___(timelineCapacity)`.

You can access any stream data aligned on time-line, or to be precise,  the most recent event prior to any time-stamp, functionally.

In the live-demo, the code access the stream data on 1 seconds prior time-stamp to the current time-stamp.

```js
var cursor = ___cursor.value(___('NOW').subtract(1, 'seconds'))
```

*Please note ___cursor is the stream data appearing while time-line proceeding to the future.*

```js
___cursor.appear(cursor);
```

Then SVG virtual DOM element is passed to react function.

As a result, you can re-play 1 seconds past world that you behaved.

```js

(function() {

  var timelineCapacity = moment.duration(10, 'seconds');

  var ___cursor = ___(timelineCapacity);

  var onMouseMove = function(e) {

    var cursor = {
      x: e.clientX,
      y: e.clientY
    };

    ___cursor.appear(cursor);
  };

  document.addEventListener("mousemove", onMouseMove);

  // here is the final part where pure logic meets our physical world
  // in lazy evaluation context, this corresponds to  `toArray()`
  ___cursor.compute(function() {});

  var Dom1 = React.createClass({
    getInitialState: function() {
      return {cursor: {x:100,y:100}};
    },
    tick: function() {
      this.setState({cursor: ___cursor.value(___('NOW').subtract(1, 'seconds'))});
    },
    componentDidMount: function() {
      this.interval = setInterval(this.tick, 10);
    },
    componentWillUnmount: function() {
      clearInterval(this.interval);
    },
    render: function() {
      return ( <div> <svg height = "100%"  width = "100%" >
      <circle cx = {  this.state.cursor.x  }  cy = {  this.state.cursor.y }  r = "20"  fill = "red" />
      </svg></div>);
    }
  });

  React.render(<Dom1 />, document.body);

  //====================================
})();


```
