<template>
  <div id="app">
    <div class="container">
      <v-card>
        <ul>
          <span>TO DO LIST</span>
          <li
            v-for="(item, idx) in listItemsFirst"
            :key="`${idx} ${item.text}`"
            class="v-info"
            draggable
            @dragstart="dragInfoStart('listItemsFirst', item)"
            @drop="dropInfo('listItemsFirst', item)"
            @dragenter="dragEnter"
            @dragover="dragOver"
            @dragleave="dragLeave"
            data-name="listItemsFirst"
          >
            {{ item.text }}
          </li>
        </ul>
      </v-card>
      <v-card>
        <ul>
          <span>CAN I ?</span>
          <li
            v-for="(item, idx) in listItemsSecond"
            :key="`${idx} ${item.text}`"
            class="v-info"
            draggable
            @dragstart="dragInfoStart('listItemsSecond', item)"
            @dragenter="dragEnter"
            @dragover="dragOver"
            @dragleave="dragLeave"
            @drop="dropInfo('listItemsSecond', item)"
            data-name="listItemsSecond"
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
      text: 'React',
      labelColor: 'blue'
    }, {
      text: 'Vue',
      labelColor: 'green'
    }, {
      text: 'angular',
      labelColor: 'red'
    }],
    listItemsSecond: [{
      text: 'React',
      labelColor: 'blue'
    }, {
      text: 'Vue',
      labelColor: 'green'
    }, {
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
    dragInfoStart (drapList, dragItem) {
      console.log(drapList)
      this.drappingInfo.from = drapList
      this.drappingInfo.item = dragItem
      console.log('start!!!')
    },
    dropInfo (dropList, item) {
      this.drappingInfo.to = dropList
      const from = this.drappingInfo.from
      const to = this.drappingInfo.to
      console.log(this[from])
      this[from] = this[from].filter(item => item !== this.drappingInfo.item)
      this[to].push(this.drappingInfo.item)
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
</style>
