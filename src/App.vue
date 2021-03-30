<template>
  <div id="app">
    <div class="row">
      <Simon />
    </div>
  </div>
</template>

<script>
import Simon from "./components/Simon.vue";

export default {
  name: "App",
  components: {
    Simon,
  },
};
</script>

<style lang="scss">
* {
  padding: 0px;
  margin: 0px;
  border: 0px;
}
*,
*:before,
*:after {
  box-sizing: border-box;
}
:focus,
:active {
  outline: none;
}
a:focus,
a:active {
  outline: none;
}
aside,
nav,
footer,
header,
section {
  display: block;
}
html,
body {
  height: 100%;
  background-color: #111;
  color: #fff;
  font-family: Arial;
}
body {
  line-height: 1;
}
input,
button,
textarea {
}
input::-ms-clear {
  display: none;
}
button {
  cursor: pointer;
}
button::-moz-focus-inner {
  padding: 0;
  border: 0;
}
a,
a:visited {
  text-decoration: none;
}
a:hover {
  text-decoration: none;
}
ul li {
  list-style: none;
}
img {
  vertical-align: top;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: inherit;
  font-size: inherit;
}
</style>
