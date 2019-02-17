# paypal-calculator

A PWA that shows a useful PayPal taxes calculator

This app was made for [#100DaysOfCode](https://www.100daysofcode.com/).

This project also belongs to #100DaysOfVue initiative.

## Additional used components

+ [v-money](https://github.com/vuejs-tips/v-money)
+ [Big.js](https://github.com/MikeMcl/big.js/)
+ [vue-pwa template](https://github.com/vuejs-templates/pwa)

## Learned during the process

1. `this.$refs` does not exist outside `methods` Vue instance.
2. [K.I.S.S Principle](https://en.wikipedia.org/wiki/KISS_principle).
3. Computed properties only make sense when they depend on another property.
4. [Props](https://vuejs.org/v2/guide/migration.html#Prop-Mutation-deprecated) _MUST_ be inmutable [by definition](https://stackoverflow.com/questions/39868963/vue-2-mutating-props-vue-warn), and only can be mutated by parent components.
5. If a prop needs to be mutated, then you _SHOULD_ use a computed property with a shallow copy of the prop value _OR_ you can change it as a [data property](https://vuejs.org/v2/guide/instance.html#Data-and-Methods).
5. Javascript sucks when it comes to maths (such as rounding numbers and currency operations).
6. [Standard.js](https://standardjs.com/) linter rules.
7. How to setup PWA manifest.json [properties](https://developer.mozilla.org/en-US/docs/Web/Manifest).
8. PWA only suports relative routing down where `manifest.json` file goes (I recommend you put into the root of your project... _PLEASE!_).
9. How to setup spaced tabs on Vim editor.

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```

### Run your tests
```
yarn run test
```

### Lints and fixes files
```
yarn run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
