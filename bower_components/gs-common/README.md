# gs-common

Componentes comunes para el desarrollo de la aplicacion Gobstones-web

###indice

* [dom-iterate](#user-content-dom-iterate) 
* [dom-inject](#user-content-dom-inject) 


##<div name="dom-iterate">dom-iterate</div>

El componente dom-iterate permite repetir un *template* HTML en un determinado rango, 
en cada iteración, dom-iterate injecta el índice sobre el *template*.


```html
<template is="dom-iterate" from="0" to="{{lastIndex}}" index-as="iterateIndex">
  <span>iterateIndex: <span>{{iterateIndex}}</span></span>
</template>
```

`from`: indice inicial. Soporta valores de modelo.

`to`: indice final. Soporta valores de modelo.

`index-as`: nombre del modelo con el que se injecta el indice en el *template*. No soporta valores de modelo.

`include-last`: Indica si se debera repetir el template con el ultimo indice del rango. 
Es un valor booleano y por limitaciones de Polymer no se puede explicitar en el html, sino que debe tomar un valor de modelo

```html
<template is="dom-iterate" from="0" to="7" include-last="includeLast">
  <span>iterateIndex: <span>{{index}}</span></span>
</template>
```

Pude ver una demostración del componente dom-iterate [aqui](http://gobstones-web.github.io/bower_components/gs-common/demo/dom-iterate/)


## <div name="dom-inject">dom-inject</div>

El componente **dom-inject** permite agregar un componente Polymer de manera dinámica.  
Aunque su mayor beneficio es aislar el comportamiento de integración y eliminación de contenido 
en un único componente. Lo que permite realizar una simple modificación ante un cambio en la implementacion de Polymer y 
su *polyfill* para soporte de *web-components*.

```js
Polymer({
  properties:{
    myElement:{
      type: Object
    }
  },

  ready: function(){
    this.storeElement = document.createElement('my-custom-element');
    this.myElement = this.storeElement;
  },

  _toggle:function(){
    if(this.myElement)
      this.myElement = null;
    else
      this.myElement = this.storeElement;
  }
});
```


```html
<dom-inject element="myElement"></dom-inject>
```

* Soporta anidamiento de componentes.
* Soporta asignacion de atributos sobre el nuevo componente.
* No soporta *binding* sobre el nuevo componente.

Puede integrarse también dentro de un *template* dom-repeat

```js
Polymer({
  properties:{
    repeatElements:{
      type: Array
    }
  },

  ready: function(){
    this.repeatElements = [];
  },
  
  _generate:function(){
    _simple = document.createElement('dom-inject-simple-example');
    this.push('repeatElements', _simple);
  },
      
  removeItem: function(evnt){
    item = evnt.target.item
    index = this.repeatElements.indexOf(item);
    this.splice('repeatElements', index, 1);
  }

});
```


```html
<template is="dom-repeat" items="{{repeatElements}}" as="simpleElement" index-as="itemIndex">
  <div class="item">
    <dom-inject element="{{simpleElement}}"></dom-inject>
  </div>
  <div class="close" on-click="removeItem" item="{{simpleElement}}">x</div>
</template>
```

Pude ver una demostración del componente dom-inject [aqui](http://gobstones-web.github.io/bower_components/gs-common/demo/dom-inject/)



