<template>
  <div id="app">
    <header>PayPal Calculator</header>
    <main>
      <img
        src="./assets/paypal-calculator-logo.svg"
        alt="Paypal Calculator PWA Logo"
      >
      <Calculator />
    </main>
    <footer>
      This app has no relation with PayPal. Inc <sup>&copy;</sup>. Its purpose is merely
      educational <br>Made by
      <a
        href="https://www.linkedin.com/in/jhonygrillet/"
        target="_blank"
      >
        Jhony Grillet
      </a>
      for
      <a
        href="https://hundreddaysofvue.github.io/"
        target="_blank"
      >
        #100DaysOfVue
      </a>
    </footer>
  </div>
</template>

<script>
import Calculator from './components/Calculator.vue'

export default {
  name: 'App',
  components: { Calculator }
}
</script>

<style>
body {
  margin: 0;
}
a,
a:link,
a:visited {
  color: white;
  font-weight: bold;
  text-decoration: underline;
}
footer,
header {
  background-color: #0044aa;
  box-sizing: border-box;
  color: #f5f5f5;
  font-weight: 400;
  height: 6rem;
  letter-spacing: 0.02rem;
  line-height: 1.2;
  text-align: center;
}
footer {
  bottom: 0;
  font-size: 1rem;
  padding: .5rem 0;
  position: relative;
}
header {
  font-size: 2rem;
  margin-bottom: 2rem;
  padding-top: 16px;
  position: relative;
}
img {
  height: 196px;
  width: 196px;
}
main {
  margin: 0 auto;
  text-align: center;
  width: 320px;
}
#app {
  color: dodgerblue;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
}
</style>
