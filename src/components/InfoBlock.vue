<template>
  <div class="v-info" v-on="handleListeners" :contenteditable="editable" :draggable="draggable">
    {{ content }}
  </div>
</template>

<script>
export default {
  name: 'info-block',
  props: {
    draggable: {
      type: Boolean,
      default: false
    },
    editable: {
      type: Boolean,
      default: false
    },
    value: String
  },
  data () {
    return {
      infoText: this.value
    }
  },
  computed: {
    content: {
      get () {
        return this.value
      },
      set (val) {
        this.infoText = val
      }
    },
    handleListeners () {
      return {
        ...this.$listeners,
        input: this.changeText,
        blur: this.blurContent
      }
    }
  },
  methods: {
    changeText (e) {
      e.preventDefault()
      this.content = e.target.textContent.trim()
    },
    blurContent () {
      this.$emit('input', this.infoText)
    }
  },
  mounted () {
  }
}
</script>

<style lang="stylus">
.v-info
  background-color #fff
  border-radius 5px
  margin-top 5px
  margin-bottom 5px
  padding-left 30px
  padding-right 30px
  padding-top 15px
  padding-bottom 15px
</style>
