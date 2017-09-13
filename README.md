# vue-xkcd
Display a random XKCD comic

## Installation

``` bash
# install with npm
npm install vue-xkcd --save
```

``` javascript
var VueXKCD = require('vue-xkcd')
Vue.use(VueXKCD)
```

## Usage

Add as a component:

``` javascript
components: {
    VueXKCD
}
```
You can then show a comic with:

``` html
<xkcd></xkcd>
```

A random image will appear wherever you place the component.