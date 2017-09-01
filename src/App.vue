<template>
  <div id="app">
    {{msg}}
    <div class="layout">
      <div class="card">
        <h4>List 1</h4>
        <draggable 
          v-model='listone'
          @start="startDrag"
          @end="endDrag"
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
            <div class="element" @click="showItem(item)" :id="item.id">{{item.name}},{{index}}</div>
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
          @add="onAdd"
          @end="endDrag"
          class="elements">
          <div v-for="(item, index) in listtwo" :key="index">
            <div class="element" @click="showItem(item)" :id="item.id">{{item.name}} , {{index}} <span class="close" @click="removeItem(index)"><strong>x</strong></span></div>
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
        {name: 'item 1', id: "ID1"},
        {name: 'item 2', id: "ID2"},
        {name: 'item 3', id: "ID3"},
        {name: 'item 4', id: "ID4"}
      ],
      listtwo: [
        {name: 'item 5', id: "ID5"},
        {name: 'item 6', id: "ID6"}
      ]
    }
  },
  // watch: {
  //   listtwo: {
  //     handler: function(newValue, oldValue){
  //       // this.diffArray(newValue, oldValue)
  //     },
  //     deep: true
  //   }
  // },
  methods: {
    removeItem: function (index) {
      this.listtwo.splice(index, 1)
    },
    showInfo: function (elem) {
      // console.log(elem)
    },
    showItem: function (elem) {
      // console.log(elem)
    },
    checkMove: function(evt) {
      // console.log(evt.draggedContext.element.id)
    },
    endDrag: function(evt) {
      if(evt.oldIndex !== evt.newIndex){
        console.log('cambio de posicion')
        console.log(evt)
        console.log(evt.clone.firstChild.id)
      } else {
        console.log('se mantiene la posicion')
      }
    },
    startDrag: function (evt) {
			// console.log(evt)
		},
    onAdd: function (evt) {
      if(confirm('Desea agregar?') == true){
        console.log(evt.clone.firstChild.id)
      } else {
        console.log('no se agrego')
        this.removeItem(evt.newIndex)
      }
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
