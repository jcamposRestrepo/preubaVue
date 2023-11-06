<template>
  <users/>

</template>

<script>
import users from "@/components/users.vue";
export default {
  name: 'HelloWorld',
  components: {
    users
  },
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

</style>
