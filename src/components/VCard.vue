<template>
  <div class="v-card">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'v-card'
}
</script>

<style lang="stylus">
.v-card
  width 200px
  min-height 100px
  background-color rgba(#fff, 0.5)
  padding 10px
  border-radius 5px
  margin-left 5px
  margin-right 5px

</style>
