<template>
  <div id="app">
    <div class="container">
      <v-card>
        <span>TO DO LIST</span>
        <ul
          style="height: 100%"
          @drop="dropInfo(arguments, 'listItemsFirst')"
          @dragover.prevent

        >
          <li
            v-for="(item, idx) in listItemsFirst"
            :key="`${idx} ${item.text}`"
            class="v-info"
            draggable
            :class="{dragging: item.isDragging}"
            @dragstart="dragInfoStart(arguments, 'listItemsFirst', item)"
          >
            {{ item.text }}
          </li>
        </ul>
      </v-card>
      <v-card>
        <span>CAN I ?</span>
        <ul
          style="height: 100%"
          @drop="dropInfo(arguments, 'listItemsSecond')"
          @dragover.prevent
        >
          <li
            v-for="(item, idx) in listItemsSecond"
            :key="`${idx} ${item.text}`"
            class="v-info"
            draggable
            :class="{dragging: item.isDragging}"
            @dragstart="dragInfoStart(arguments, 'listItemsSecond', item)"
          >
            {{ item.text }}
          </li>
        </ul>
      </v-card>
    </div>
  </div>
</template>

<script>
import VCard from '@/components/VCard'
export default {
  name: 'app',
  components: {
    VCard
  },
  data: () => ({
    listItemsFirst: [{
      id: 1,
      text: 'React',
      labelColor: 'blue',
      isDragging: false
    }, {
      id: 2,
      text: 'Vue',
      labelColor: 'green'
    }, {
      id: 3,
      text: 'angular',
      labelColor: 'red'
    }],
    listItemsSecond: [{
      id: 4,
      text: 'React',
      labelColor: 'blue'
    }, {
      id: 5,
      text: 'Vue',
      labelColor: 'green'
    }, {
      id: 6,
      text: 'angular',
      labelColor: 'red'
    }],
    drappingInfo: {
      from: '',
      to: '',
      item: {}
    }
  }),
  methods: {
    dragInfoStart (args, from, dragItem) {
      const dragEvent = args[0]
      
      dragEvent.dataTransfer.setData('text/plain', `${from},${dragItem.id}`)
      this.$set(dragItem, 'isDragging', true)
      console.log(dragItem)
      console.log(dragEvent)
    },
    dropInfo (args, to) {
      const dragEvent = args[0]
      dragEvent.preventDefault
      const nodeList = [...dragEvent.path[1].childNodes]
      const dropNode = dragEvent.path[0]
      const insertIndex = nodeList.indexOf(dropNode) + 1
      const [from, id] = dragEvent.dataTransfer.getData('text').split(',')
      const activeItem = this[from].find(item => item.id + '' === id)
      this.$set(activeItem, 'isDragging', false)
      this[from] = this[from].filter(item => item.id + '' !== id)
      this[to].splice(insertIndex, 0, activeItem)
    },
    // @drop="drop"
    drop (e) {
      console.log('drop!' + e)
    },
    dragEnter (e) {
      console.log('dragEnter!' + e)
    },
    dragOver (e) {
      e.preventDefault()
      console.log('dragOver' + e)
    },
    dragLeave (e) {
      console.log('drag leave' + e)
    }
    // @dragenter="dragEnter"
    // @dragover="dragOver"
    // @dragleave="dragLeave"
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
.v-info
  background-color #fff
  border-radius 5px
  margin-top 5px
  margin-bottom 5px
  padding-left 30px
  padding-right 30px
  padding-top 15px
  padding-bottom 15px
.dragging
  background-color rgba(#EAEAEA, 0.5)
</style>
