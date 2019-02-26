<template>
  <div id="app">
    <div class="container">
      <v-card>
        <span>TO DO LIST</span>
        <ul
          class="card__list"
          @drop="dropInfo(arguments, 'listItemsFirst')"
          @dragover.prevent
          @dragenter="dragEnter(arguments, 'listItemsFirst')"

        >
          <info-block
            @dblclick="item.editable = !item.editable"
            v-for="(item, idx) in listItemsFirst"
            :key="`${idx} ${item.text}`"
             v-model="item.text"
            :editable="item.editable"
            draggable
            :class="{dragging: item.isDragging}"
            @dragstart="dragInfoStart(arguments, 'listItemsFirst', item)"
          >
            {{ item.text }}
          </info-block>
        </ul>
        <a class="add__list" @click="addInfo(arguments, 'listItemsFirst')">+ 新增項目</a>
      </v-card>
      <v-card>
        <span>CAN I ?</span>
        <ul
          class="card__list"
          @drop="dropInfo(arguments, 'listItemsSecond')"
          @dragenter="dragEnter(arguments, 'listItemsSecond')"
          @dragover.prevent

        >
          <info-block
            @dblclick="item.editable = !item.editable"
            v-for="(item, idx) in listItemsSecond"
            :key="`${idx} ${item.text}`"
            v-model="item.text"
            :editable="item.editable"
            draggable
            :class="{dragging: item.isDragging}"
            @dragstart="dragInfoStart(arguments, 'listItemsSecond', item)"
          >
            {{ item.text }}
          </info-block>
        </ul>
        <a class="add__list"  @click="addInfo(arguments, 'listItemsSecond')">+ 新增項目</a>
      </v-card>
    </div>
  </div>
</template>

<script>
import VCard from '@/components/VCard'
import InfoBlock from '@/components/InfoBlock.vue'
console.log(VCard)
export default {
  name: 'app',
  components: {
    VCard,
    InfoBlock
  },
  data: () => ({
    listItemsFirst: [{
      id: 1,
      text: 'React',
      labelColor: 'blue',
      isDragging: false,
      editable: false
    }, {
      id: 2,
      text: 'Vue',
      labelColor: 'green',
      editable: false
    }, {
      id: 3,
      text: 'angular',
      labelColor: 'red',
      editable: false
    }],
    listItemsSecond: [{
      id: 4,
      text: 'React',
      labelColor: 'blue',
      editable: false
    }, {
      id: 5,
      text: 'Vue',
      labelColor: 'green',
      editable: false
    }, {
      id: 6,
      text: 'angular',
      labelColor: 'red',
      editable: false
    }],
    draggingInfo: {
      from: '',
      to: '',
      item: {}
    }
  }),
  methods: {
    addInfo (args, name) {
      const dragEvent = args[0]
      dragEvent.preventDefault()
      this[name].push({
        id: Date.now(),
        text: 'Hello' + Date.now(),
        labelColor: 'blue',
        editable: false
      })
    },
    dragInfoStart (args, from, dragItem) {
      console.log('fk')
      this.draggingInfo.from = from
      this.draggingInfo.item = dragItem
      this.$set(dragItem, 'isDragging', true)
    },
    dropInfo (args, to) {
      this.$set(this.draggingInfo.item, 'isDragging', false)
    },
    dragEnter (args, to) {
      console.log('dragEnter!')
      this.draggingInfo.to = to
      const dragEvent = args[0]
      if (!dragEvent.target.className.includes('v-info')) return
      dragEvent.preventDefault()
      const nodeList = [...dragEvent.path[1].childNodes]
      const dropNode = dragEvent.path[0]
      const insertIndex = nodeList.indexOf(dropNode)
      const { item, from } = this.draggingInfo
      this[from] = this[from].filter(card => card.id !== item.id)
      this[to].splice(insertIndex, 0, item)
      this.draggingInfo.from = to
    }
  }
}
</script>
<style lang="stylus">
*
  box-sizing border-box
  padding 0px
  margin 0px
  list-style-type none
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  background-color #0079BF
  height 100vh
.container
  padding-right 50px
  padding-left 50px
  padding-top 30px
  display flex

.dragging
  background-color rgba(#EAEAEA, 0.5)
.add__list
  padding-top 10px
  color rgba(#fff, .75)
  cursor pointer
  &:hover
    color rgba(#fff, 0.3)

</style>
