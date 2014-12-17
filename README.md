react-enquire
=============

Monadic usage of enquire.js in React 

That component is complex using of React Reflux and Fantasyland, that wraped behavior enquire.

## Example

Basicaly you need to register handler in top level component such as ```Application```.

```javascript
var Enquire = require('react-enquire');

var Application = React.createClass({
  mixins : [
    Enquire.createHandler({
      visibleXs : 'max-width(479px)',
      visibleSm : 'min-width(480px) and max-width(767px)',
      visibleMd : 'min-width(768px) and max-width(991px)',
      visibleLg : 'min-width(992px) and max-width(1199px)',
      visibleXg : 'min-width(1200px)',
    }),
  ]
});

```

After that you may use all keys as state variables after connect to component Store from module. 
Motivation: ```VeryComplexComponent``` has too many computation that not needed in mobile view.

```javascript
var Reflux = require('reflux'),
    Enquire = require('react-enquire'),
    Option = require('fantasy-options').Option,
    foldable = require('react-fantasy').foldable;

var SomeComponent = React.createClass({
  mixins : [
    Reflux.ListenerMixin,
    Reflux.connect(Enquire.store)
  ],
  
  renderVeryComplexComponent : function () {
    return (
      <VeryComplexComponent />
    )
  },
  
  render : function() {
    return foldable().then(this.renderNull).fail(this.renderVeryComplexComponent).exec(this.state.visibleXs);
  }
})

var XsComponent = React.createClass({
  mixins : [
    Reflux.ListenerMixin,
    Reflux.listenTo(Enquire.store, onMediaChanged)
  ],
  
  onMediaChanged : function(mediaMathches) {
    mediaMatches.visibleXs.isNot(this.state.visibleXs).map(Option.of).chain(this.updateState)
  },
  
  updateState : function(visibleXs) {
    this.setState({
      visibleXs : visibleXs
    });
  }
  
  renderVeryComplexComponent : function () {
    return (
      <VeryComplexComponent />
    )
  },
  
  render : function() {
    return foldable().then(this.renderNull).fail(this.renderVeryComplexComponent).exec(this.state.visibleXs);
  }
})

```

In this example ```XsComponent``` will react only for changes in ```visibleXs``` media query and will ignore others.
