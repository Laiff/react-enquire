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
      xs : 'max-width(479px)',
      sm : 'min-width(480px) and max-width(767px)',
      md : 'min-width(768px) and max-width(991px)',
      lg : 'min-width(992px) and max-width(1199px)',
      xg : 'min-width(1200px)',
    }),
  ]
});

```

After that you may use all keys as state variables after connect to component Store from module. 
Motivation: ```VeryComplexComponent``` has too many computation that not needed in mobile view.

```javascript
var Reflux = require('reflux'),
    Enquire = require('react-enquire'),
    option = require('react-fantasy').option;


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
    return option().withFallback(this.renderVeryComplexComponent).render(this.state.xs);
  }
})

```

