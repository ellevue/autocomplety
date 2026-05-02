# v-autocomplete with "other" option

Este paquete permite usar el componente v-autocomplete de vuetify con la opción "other".

## Requerimientos

* [vuetify](https://vuetifyjs.com/en/)

## Instalación

```bash
npm install @ellevue/autocomplety
```

## Uso

```js
import VAutocompleteExtended from '@ellevue/autocomplety';
Vue.component('v-autocomplete-extended', VAutocompleteExtended);
```

```html
<v-autocomplete-extended other="Otro número" :rules="[$rules.required]" label="Números" v-model='["uno", "dos", "tres"]'></v-autocomplete-extended>
```