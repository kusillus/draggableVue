<template>
  <div id="app">
    {{msg}}
    <div class="layout">
      <div class="card">
        <h4>List 1</h4>
        <draggable 
          v-model='listone' 
          :options="{
            group: {
              name: 'items',
              pull: 'clone',
              put: false
            }
          }"
          class="elements"
          @click="showInfo()">
          <div v-for="(item, index) in listone" :key="index">
            <div class="element" @click="showItem(item)">{{item.name}},{{index}}</div>
          </div>
        </draggable>
        <button @click="showInfo(listone)">SendListOne</button>
      </div>
      <div class="card">
        <h4>List 2</h4>
        <draggable
          :list="listtwo"
          v-model='listtwo'
          :options="{group:'items'}"
          :move="checkMove"
          @end="endDrag"
          class="elements">
          <div v-for="(item, index) in listtwo" :key="index">
            <div class="element" @click="showItem(item)">{{item.name}} , {{index}} <span class="close" @click="removeItem(index)"><strong>x</strong></span></div>
          </div>
        </draggable>
        <button @click="showInfo(listtwo)">SendListTwo</button>
      </div>
    </div>

  
  </div>
</template>

<script>
import draggable from 'vuedraggable'
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      listone: [
        {name: 'item 1', id: 1},
        {name: 'item 2', id: 2},
        {name: 'item 3', id: 3},
        {name: 'item 4', id: 4}
      ],
      listtwo: [
        {name: 'item 5', id: 5},
        {name: 'item 6', id: 6}
      ]
    }
  },
  watch: {
    listtwo: {
      handler: function(newValue, oldValue){
        this.diffArray(newValue, oldValue)
        // console.log(newValue)
      },
      deep: true
    }
  },
  methods: {
    removeItem: function (index) {
      // this.listtwo.splice(index, 1)
    },
    showInfo: function (elem) {
      // console.log(elem)
    },
    showItem: function (elem) {
      // console.log(elem)
    },
    diffArray: function(aNew, aOld) {
      if(aNew.length != aOld.length){
        // aNew.forEach(function(element, index) {
        //   // console.log(aNew[index].id)
        //   aOld.forEach(function(oldElem, oldIdx){
        //     console.log(aOld[oldIdx].id)
        //     // if(aNew[index].id == )            
        //   })
        //   // console.log(index)
        //   // Object.keys(element).forEach(function (item){
        //   //   console.log(item)
        //   // })
        // })
      } else {
        // console.log('Equals!')
      }
      
    },
    checkMove: function(evt) {
      return (evt.draggedContext.element.name)
    },
    endDrag: function(evt) {
      console.log(evt)
    }
  },
  components: {
    draggable
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
.layout {
  width: 100%;
  display: flex;
  justify-content: space-around;
}
.card {
  width: 150px;
  // border: solid .1rem gray;
  // border-radius: .2rem;
  // padding: 1rem; 
}
.elements {
  cursor: pointer;
  min-height: 2rem;
  padding: 1rem;
  border: solid .1rem gray;
  border-radius: .2rem;
}
.element {
  padding: .5rem .2rem;
}
.close {
  // border: solid .1rem gray;
  border-radius: .2rem;
  background-color: gray;
  color: white;
  padding: 0 .3rem;
}
h4 {
  margin: 0;
  text-align: left;
}
</style>
