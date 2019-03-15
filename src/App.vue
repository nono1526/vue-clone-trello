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
            :class="{dragging: item.dragging}"
            @dragstart="dragInfoStart('listItemsFirst', item)"
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
            :class="{dragging: item.dragging}"
            draggable
            @dragstart="dragInfoStart('listItemsSecond', item)"
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
      dragging: false,
      editable: false
    }, {
      id: 2,
      text: 'Vue',
      dragging: false,
      editable: false
    }, {
      id: 3,
      text: 'angular',
      dragging: false,
      editable: false
    }],
    listItemsSecond: [{
      id: 4,
      text: 'React',
      dragging: false,
      editable: false
    }, {
      id: 5,
      text: 'Vue',
      dragging: false,
      editable: false
    }, {
      id: 6,
      text: 'angular',
      dragging: false,
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
        text: 'dblclick edit',
        labelColor: 'blue',
        editable: false
      })
    },
    dragInfoStart (from, dragItem) {
      this.draggingInfo.from = from
      this.draggingInfo.item = dragItem
      dragItem.dragging = true
    },
    dropInfo (args, to) {
      this.draggingInfo.item.dragging = false
    },
    dragEnter (args, to) {
      this.draggingInfo.to = to
      const [dragEvent] = args
      if (!dragEvent.target.className.includes('v-info')) return
      dragEvent.preventDefault()
      const nodeList = [...dragEvent.path[1].childNodes]
      const dropNode = dragEvent.path[0]
      // 找到目前拖曳所在位置
      const insertIndex = nodeList.indexOf(dropNode)
      const { item, from } = this.draggingInfo
      this[from] = this[from].filter(card => card.id !== item.id)
      // 插入到所在位置
      this[to].splice(insertIndex, 0, item)
      // 因為使用者還沒有放開資訊卡，所以需把最後的欄位在紀錄起來！
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
