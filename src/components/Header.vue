<template>
  <div class="cls_header">
    <router-link to = "/" exact-active-class = "_active">Home</router-link>
  </div>
</template>

<script>
export default {
  data () {
    return {
    }
  }
}
</script>

<style>
.cls_header a {
    display: inline-block;
    font-size: 24px;
    color: #ffffff;
    padding: 20px;
}
div#app, body {
    margin: 0;
}
.cls_header {
    background-color: #000000;
    padding: 30px;
}
.bodyClass{
  margin: auto;
  max-width: 1024px;
}
img.cls_icon {
    width: auto;
    display: inline-block;
    vertical-align: middle;
}
img.cls_search {
    max-width: 25px;
    display: inline-block;
    vertical-align: middle;
}
</style>
