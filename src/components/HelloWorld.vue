<template>
  <div class="demo">
    <item/>
  </div>
</template>

<script>
import item from "./Item"
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  components: {
    item,
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .demo {
    margin: 0 auto;
    width: 100%;
    height: 100%;
    background-color: #f5f5f5;
    position: absolute;
  }
</style>
