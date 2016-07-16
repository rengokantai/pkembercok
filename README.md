#### pkembercok
#####Chapter 1. Ember CLI Basics
######Upgrading your project
only upgrade ember-cli version
```
npm uninstall –g ember-cli
npm cache clean
bower cache clean
npm install –g ember-cli
```
exsiting project.
```
rm –rf node_modules bower_components dist tmp
npm install ember-cli@X.X.X --save-dev
npm install
bower install
ember init
```
#####Chapter 2. The Ember.Object Model
######Working with computed properties
```

const Light = Ember.Object.extend({
  isOn:null,
  color:'yellow',
  description:Ember.computed('isOn',function(){
    return this.get('isOn')+'123';
  }),
  a:Ember.computed.alias('description'),
  isOnChanged: Ember.observer('isOn',function() {
    console.log('isOn value changed')
  }),
  //Or
  isAnythingChanged: Ember.observer('isOn','color',function() {
    console.log('isOn or color value changed')
  })
});

const light = Light.create({isOn:1})
console.log(light.get('a'))
light.set('isOn',true);
```

Ember.run.once (solve sync issue)
```
 Light.reopen({
    checkIsOn: Ember.observer('isOn', function() {
      console.log(this.get('fullDescription'));
    })
  });
```
but we do not know whether fullDescription has changed
```
Light.reopen({
    checkIsOn: Ember.observer('isOn','color', function() {
      Ember.run.once(this,'checkChanged');
    }),
    checkChanged: Ember.observer('description',function() {
      console.log(this.get('description'));
    })
});
```
######Working with bindings
Ember.computed.oneWay
```
const User = Ember.Object.extend({
  firstName: null,
  lastName: null,
  nickName: Ember.computed.oneWay('firstName')
});

const user = User.create({
  firstName: 'Erik',
  lastName:  'Hanchett'
});

console.log(user.get('nickName'));              // 'Erik'
user.set('nickName', 'Bravo'); // 'Bravo'   Does not change original value
console.log(user.get('firstName'));             // 'Erik'
```
######Using mixins
```
const a = Ember.Mixin.create({a:'a'});
const b = Ember.Object.extend(a,{});
```
use ember-cli
```
ember generate mixin a
```
