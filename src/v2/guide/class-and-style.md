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

Netijede alarys:

``` html
<div class="active text-danger"></div>
```

Eger-de class-yň bahalaryny şertli üýtgeýän etmek isleseňiz, onda siz şertli aňlatmalary berip bilersiňiz:

``` html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

This will always apply `errorClass`, but will only apply `activeClass` when `isActive` is truthy.
Bu `errorClass`-y hemişe, `activeClass`-y bolsa haçanda `isActive` dogry bolsa class-yň bahasyna goşar.

Eger-de birnäçe şertli class-yň bahalary bar bolsa, ýokarky görnüşde ýazmak bulaşyklyga getirip biler. Şonuň üçin hem, array-iň içinde object görnüşi hem ulanmak mümkin:

``` html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### Component-ler bilen ulanylyşy

> Bu bölüme düşinmek üçin [Vue Component](components.html) bilen tanyş bolmak hökmanydyr. Eger-de nätanyş bolsaňyz, ol bölümi geçip, soňra dowam edip bilersiňiz. 

Eger-de `class` attributyny hususy component-iňizde ulansaňyz, şol class bahalary component-iň kök elementini goşular. Öňden bar bolan class bahalary çalyşylmaýar.

Meselem, şu component-a:

``` js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

Ulanylanda käbir class bahalary goşulanda:

``` html
<my-component class="baz boo"></my-component>
```

Şu HTML görkeziler:

``` html
<p class="foo bar baz boo">Hi</p>
```

Edil şuňa meňzeşlikde, class-lar üçin hem şeýledir:

``` html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

Eger-de `isActive` dogry bolsa, görkeziljek HTML şeýledir:

``` html
<p class="foo bar active">Hi</p>
```

## Içki Style-lary üýtgetmek

### Object görnüşinde

`v-bind:style`-a object baha deňlemek örän ýeňil we ol JavaScript object-ligine garamazdan, edil CSS ýalydyr. CSS bahalaryny camelCase ýa-da kebab-case (kebab-case-da bir dyrnak goýmagy unutmaň) bilen berip bilersiňiz:

``` html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
``` js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

Köplenç style-yň bahasyny, data-da bar bolan object-e deňlemek gowy pikirdir we kody arassa görkezýär:

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

Object görnüş köplenç, object gaýtaryp berýän computed häsiýet bilen hem ulanylyp biliner.

### Arraý görnüşinde

`v-bind:style` üçin array görnüşinde style bermek, şol bir elemente birnäçe style object-ni bermäge mümkinçilik berýär:

``` html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### Auto-prefixing Awto prefikslemek

Haçanda, siz `v-bind:style`-da [vendor prefiks](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix) hökmany bolan CSS bahasyny ulansaňyz, meselem: `transform`, Vue awtomatiki usulda prefiks-i kesgitleýär we gerek bolan style-yň bahasyna goşýar.

### Birnäçe bahalar

> 2.3.0+

2.3.0+ wersiýadan başlap, birnäçe bahalary array bilen (prefikslenen) style-a berip bilersiňiz. Meselem:

``` html
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

Bu diňe browseriň goldaýan, array-iň iň soňky elementi görkezer. Bu meselemde, flexbox-yň prefikssiz wersiýasyny goldaýan browserler üçin `display: flex` görkezer.
