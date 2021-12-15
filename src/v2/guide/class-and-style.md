---
title: Class we Style üýtgetmek
type: guide
order: 6
---

Köplenç ýagdaýda elemente berilen içki style we class-lary üýtgeýän etmek zerurlygy ýüze çykýar. Olaryň ikisi hem attribut bolany üçin, olary `v-bind` bilen üýtgetmek mümkin: bize diňe aňlatmanyň netijesiniň string bolmagy zerur. Şeýle-de bolsa, muňa garamazdan string bilen işlemek ýürege düşiji we käbir ýagdaýlarda ýalňyşlyga ýol açyp bilýär. Şol sebäpli hem, Vue `v-bind` bilen `class` we `style` attributlaryny ulanmak üçin ýörite mümkinçilikler berýär. String-den başga-da aňlatmalar object we array görnüşinde hem bolup biler.

## HTML Class-laryny üýtgetmek
<div class="vueschool"><a href="https://vueschool.io/lessons/vuejs-dynamic-classes?friend=vuejs" target="_blank" rel="sponsored noopener" title="Free Vue.js Dynamic Classes Lesson">Vue School-da muzdsuz wideo sapagyna görip bilersiňiz</a></div>

### Object görnüşinde

Class-yň bahasyna, üýtgäp bilýän bahany bermek üçin `v-bind:class`-a object deňläp bilers.

``` html
<div v-bind:class="{ active: isActive }"></div>
```

Ýokardaky aňlatmada, eger-de `isActive` [dogry](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) bolsa, class `active` bahany alar.

Class-y birnäçe bahasy bolan object-e hem deňläp bilersiňiz. Mundan başga-da, `v-bind:class` direktiwasyny `class` attributy bilen bilelikde ulanyp bilersiňiz:

``` html
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
```

Şu maglumatlar bilen:

``` js
data: {
  isActive: true,
  hasError: false
}
```

Netijede alarys:

``` html
<div class="static active"></div>
```

Haçanda `isActive` ýa-da `hasError` üýtgese, class-yň bahalary hem degişlilikde üýtgär. Meselem, eger-de `hasError` `true` baha üýtgese class-yň bahasy `"static active text-danger"` deň bolar.

Classyň bahasy data-da bar bolan object-iň adyna hem deň bolup biler:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

Bu hem şol bir netijäni berer. Class-y object gaýtaryp berýän [computed](computed.html) häsiýet bilen hem deňläp bilers. Bu örän güýçli tehnika:

``` html
<div v-bind:class="classObject"></div>
```
``` js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

### Array görnüşinde

Class-a bahalary bermek üçin, `v-bind:class`-a array bermek mümkin:

``` html
<div v-bind:class="[activeClass, errorClass]"></div>
```
``` js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

Which will render:

``` html
<div class="active text-danger"></div>
```

If you would like to also toggle a class in the list conditionally, you can do it with a ternary expression:

``` html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

This will always apply `errorClass`, but will only apply `activeClass` when `isActive` is truthy.

However, this can be a bit verbose if you have multiple conditional classes. That's why it's also possible to use the object syntax inside array syntax:

``` html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### With Components

> This section assumes knowledge of [Vue Components](components.html). Feel free to skip it and come back later.

When you use the `class` attribute on a custom component, those classes will be added to the component's root element. Existing classes on this element will not be overwritten.

For example, if you declare this component:

``` js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

Then add some classes when using it:

``` html
<my-component class="baz boo"></my-component>
```

The rendered HTML will be:

``` html
<p class="foo bar baz boo">Hi</p>
```

The same is true for class bindings:

``` html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

When `isActive` is truthy, the rendered HTML will be:

``` html
<p class="foo bar active">Hi</p>
```

## Binding Inline Styles

### Object Syntax

The object syntax for `v-bind:style` is pretty straightforward - it looks almost like CSS, except it's a JavaScript object. You can use either camelCase or kebab-case (use quotes with kebab-case) for the CSS property names:

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

It is often a good idea to bind to a style object directly so that the template is cleaner:

``` html
<div v-bind:style="styleObject"></div>
```
``` js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

Again, the object syntax is often used in conjunction with computed properties that return objects.

### Array Syntax

The array syntax for `v-bind:style` allows you to apply multiple style objects to the same element:

``` html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### Auto-prefixing

When you use a CSS property that requires [vendor prefixes](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix) in `v-bind:style`, for example `transform`, Vue will automatically detect and add appropriate prefixes to the applied styles.

### Multiple Values

> 2.3.0+

Starting in 2.3.0+ you can provide an array of multiple (prefixed) values to a style property, for example:

``` html
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

This will only render the last value in the array which the browser supports. In this example, it will render `display: flex` for browsers that support the unprefixed version of flexbox.
