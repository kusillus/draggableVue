<template>
  <div id="app">
    {{msg}}
    <div class="layout">
      <div class="card">
        <h4>List 1</h4>
        <draggable          
          v-model='listone'
          @end="endDrag"
          :options="{
            group: {
              name: 'items',
              pull: 'clone',
              put: ['inf']
            }
          }"
          class="elements">
          <div v-for="(item, index) in listone" :key="index" data-type="lExterna">
            <div class="element" :data-name="item.name" :id="item.id">{{item.name}},{{index}}</div>
          </div>
        </draggable>
      </div>
      <div>
        <h5 style="margin:0;">Lista Definitiva</h5>
        <draggable
          :list="listAn"
          :options="{ group:'Padre',
                      name: 'sup',
                      put:false}"
          @add="onAdd"
          :move="checkMove"
          class="elements">
          <div class="elements" style="margin:.4rem;" v-for="(item, index) in listAn" :key="index">
            <div v-if="item.name != 'new_list'">
              <h5 style="margin:0;">Lista {{item.name}} <span class="close" @click="removeItem(index)"><strong>x</strong></span></h5>
                <draggable
                  :list="item.values"
                  :move="moveItem"
                  @end="endDrag"
                  :options="{ group:'items',
                              name:'inf',
                              put:['sup']
                              }">
                  <div v-for="(itm, idx) in item.values" :key="idx">
                    <div style="padding:.5rem;" :id="itm.id" :data-name="itm.name" data-type="lInterna"  :data-title="item.name">{{itm.name}} , {{idx}} <span class="close" @click="removeItemAn(idx, index)"><strong>x</strong></span></div>
                  </div>
                </draggable>  
            </div>
            <div v-else>
              <h5 style="margin:0;">Lista {{item.name}}</h5>
              <draggable
                :list="item.values"
                v-model='item.values'
                :move="noMove"
                :options="{group:'items'}"
                @add="addNew"
                class="elements bg-red"> 
              </draggable>
            </div>
          </div>
        </draggable>
      </div>
      <!-- ESTA ES UNA LISTA DE PRUEBA -->
      <div>
        <h5>Lista de prueba</h5>
        <pre>
          {{ listAn }}
        </pre>
        <pre>
          {{ listone }}
        </pre>
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
      ],
      listAn: [
        { name: 'new_list',
          values: [{}]
        },
        { name: 'List1',
          values: [
            {name: 'item UNO', id: "ID7"},
            {name: 'item DOS', id: "ID8"}
          ]  
        },
        { name: 'List2',
          values: [
            {name: 'item TRES', id: "ID9"},
            {name: 'item CUATRO', id: "ID10"}
          ]  
        },
        { name: 'List3',
          values: [
            {name: 'item CINCO', id: "ID11"},
            {name: 'item SEIS', id: "ID12"}
          ]  
        }
      ]
    }
  },
  watch: {

  },
  methods: {
    removeItem: function (index) {
      this.listAn.splice(index, 1)
    },
    removeItemAn: function (idx, supIdx) {
      console.log('Eliminado!')
      console.log(idx)
      console.log(supIdx)
      if(this.listAn[supIdx].values.length == 1) {
        this.listAn.splice(supIdx,1)
      } else {
        this.listAn[supIdx].values.splice(idx, 1)
      }
    },
    moveItem: function () {
      return true
    },
    noMove: function(){
      return false
    },
    endDrag: function(evt) {  
      let inText = evt.srcElement.innerText.length
      console.log(inText)
      if(inText == 0) {
        let eIndex
        let lName = evt.item.firstChild.dataset.title
        console.log(lName)
        this.listAn.map(function(item, index){
          if(item.name == lName){
            eIndex = index
          }
        })
        console.log(inText)
        this.listAn.splice(eIndex,1)

      }
    },
    checkMove: function(evt) {
      console.log(evt.draggedContext.element.name)
      if(evt.draggedContext.element.name == "new_list"){
        return false
      }
    },
    pack: function (evt) {
      console.log(evt)
    },
    onAdd: function (evt) {
      this.listAn.splice(evt.newIndex, 1)
    },
    addNew: function (el) {
      // console.log(el.clone.firstChild.dataset.name)
      // console.log(el.clone.firstChild.id)
      this.listAn.push({
        name: el.clone.firstChild.dataset.name,
        values: [
            {name: el.clone.firstChild.dataset.name, id: el.clone.firstChild.id}
          ]
      })
      let iNewL
      this.listAn.map(function(item, index) {
        if(item.name == 'new_list'){
          iNewL = index
        }
      })
      this.listAn[iNewL].values.splice(0,1)
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
  border-radius: .2rem;
  background-color: gray;
  color: white;
  padding: 0 .3rem;
}
h4 {
  margin: 0;
  text-align: left;
}
.bg-red {
  background-color: red;
}
</style>
