<template>
  <div class="fluid container">
    <div class="form-group form-group-lg panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Sortbale control</h3>
      </div>
      <div class="panel-body">
        <div class = "checkbox">
          <label><input type = "checkbox" v-model="editable">Enable drag and drop</label>      
        </div>
        <button type="button" class="btn btn-default" @click="orderList">Sort by original order</button>
        <button type="button" class="btn btn-default" @click="mttest('123','aaa')">Sort by original order</button>
      </div>
    </div>
<br/><br/><br/><br/>
<div v-for=" item in files" @click="addFile(item.Id)">{{item.Title}}
<div v-for=" item2 in item.arr">{{item2.Title}}</div>
</div>

<br/><br/>
<item-tree :model='model' v-for='model in files'></item-tree>
<br/><br/>
    <div>
      <draggable element="div" :options="dragOptions2" :move="onMove2" @start="startLink($event)" @end="endLink($event)"> 
          <transition-group type="transition" :name="'flip-link'" tag="div">
            <div class="list-group-item col-md-2 col-md-offset-1" v-for="element in link" :key="element.id" :id="element.id"> 
              <div   aria-hidden="false">
              {{element.title}}
              <br/><br/>
                <draggable class="list-group" element="ul" v-model="element.list" :options="dragOptions" :move="onMove" @start="startActivity($event)" @end="endActivity($event,element.id)" @change="updateActivity($event,element.id)"> 
                  <transition-group type="transition" class="list-group"  name="on" tag="ul">
                    <li class="list-group-item" v-for="ele in element.list" :key="ele.id" :id="ele.id" :fatherId="element.id"> 
                     <i :class="[ele.fixed? 'fa fa-anchor' : 'glyphicon glyphicon-pushpin']"  aria-hidden="true"></i>
                      {{ele.name}}
                      <span class="badge">{{ele.id}}</span>
                    </li> 
                  </transition-group>
                </draggable>

                1231312
              </div>
            </div> 
          </transition-group>
      </draggable>
    </div>
    <br/>
     <br/>
     <hr> 
  </div>
</template>

<script>
import draggable from 'vuedraggable'
import itemTree from './itemTree'
const message = [ 'vue.draggable', 'draggable', 'component', 'for', 'vue.js 2.0', 'based' , 'on', 'Sortablejs' ]
const meg2 = [{title:"wangshuai", id: 1, fixed2: false, list: [
{name:"1", id: 0, fixed: false, ty: false},
{name:"vuegable", id: 1, fixed: false, ty: true},
{name:"draggable", id: 2, fixed: false, ty: true},
{name:"componen", id: 3, fixed: false, ty: true},
{name:"forww", id: 4, fixed: false, ty: true}
]}, {title:"zhangguoxi", id: 2, fixed2: false, list:[
{name:"2", id: 5, fixed: false, ty: false},
{name:"vue.js 2.0", id: 6, fixed: false, ty: true},
{name:"basedxixi", id: 7, fixed: false, ty: true}
]}, {title:"yujun", id: 3, fixed2: false, list:[
{name:"3", id: 8, fixed: false, ty: false},
{name:"Sortablejs", id: 9, fixed: false, ty: true}
]}, {title:"wagnxulai", id: 4, fixed2: false, list:[
{name:"4", id: 10, fixed: false, ty: false},
{name:"hhhhhhhh", id: 11, fixed: false, ty: true}
]}]
export default {
  name: 'hello',
  components: {
    draggable,itemTree
  },
  data () {
    return {
      link:meg2,
      list: message.map( (name,index) => {return {name, order: index+1, fixed: false}; }),
      list2:[],
      list3:[],
      editable:true,
      isDragging: false,
      delayedDragging:false,
      isDragging2: false,
      delayedDragging2:false,
      oldLinkId:0,
      newLinkId:0,
      chooiseLinkId:0,
      files:[{"Id":"0","Title":"wang","arr":[{"Id":"101","Title":"wangshuai","arr":[]}]},{"Id":"1","Title":"zhang","arr":[]}]
    }
  },
  methods:{
    addFile (id){
      console.log(id)
      if(id==1){
        this.files[id].attr = [{"Id":"102","Title":"yujun","arr":[]}]
        this.files = Object.assign({},this.files)
        return this.files
      }
    },
    mttest (obj1, obj2) {
       console.log(obj1)
       console.log(obj2)
    },
    orderList () {
      this.list = this.list.sort((one,two) =>{return one.order-two.order; })
    },
    onMove ({relatedContext, draggedContext}) {
      const relatedElement = relatedContext.element;
      const draggedElement = draggedContext.element;
      return (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
    },
    onMove2 ({relatedContext, draggedContext}) {
      const relatedElement = relatedContext.element;
      const draggedElement = draggedContext.element;
      return (!relatedElement || !relatedElement.fixed2) && !draggedElement.fixed2
    },
    startLink (event) {
      this.isDragging2=true
      //console.log(event.item)
    },
    endLink (event){
       this.isDragging2=false
       console.log(event.target.childNodes[event.oldIndex].id) //ExchangeLinkId
       console.log(event.item.id)   // CurrentLinkId
    },
    startActivity (event) {
      this.isDragging=true
      //console.log(event.item)
    },
    endActivity (event,id){
       this.isDragging=false
       console.log(event.newIndex)
       console.log(event.oldIndex)
       console.log(event.item.id)   //CurrentActivityId
       if(this.chooiseLinkId == id){
         console.log(id)
       }
       else {
         console.log(this.chooiseLinkId)
       }
       this.newLinkId = 0
       this.oldLinkId = 0
    },
    updateActivity (event,id){
      //console.log(event)
      if(this.oldLinkId == 0){
        this.oldLinkId = this.oldLinkId+1
      }
      else {
        this.newLinkId = this.newLinkId+1
      }
      if(this.oldLinkId != this.newLinkId)  {
        this.chooiseLinkId = id
        //console.log('------------')
        //console.log(this.oldLinkId)
        //console.log('------------')
        //console.log(id)
        //console.log('------------')
      }
    }
  },
  computed: {
    dragOptions () {
      return  {
        animation: 0,
        group: 'description',  //不同的group,拖拽层级不同
        disabled: !this.editable,
        ghostClass: 'ghost'
      };
    },
    dragOptions2 () {
      return  {
        animation: 0,
        group: 'description2',
        disabled: !this.editable,
        ghostClass: 'ghost'
      };
    }
  },
  watch: {
    isDragging (newValue) {
      if (newValue){
        this.delayedDragging= true
        return
      }
      this.$nextTick( () =>{
           this.delayedDragging =false
      })
    },
    isDragging2 (newValue) {
      if (newValue){
        this.delayedDragging2= true
        return
      }
      this.$nextTick( () =>{
           this.delayedDragging2 =false
      })
    }
  }
}
</script>

<style>
.flip-link-move {
  transition: transform 0.5s;
}
.flip-list-move {
  transition: transform 0.5s;
}

.no-move {
  transition: transform 0s;
}

.ghost {
  opacity: .5;
  background: #C8EBFB;
}

.list-group {
  min-height: 50px;
  //sortable-chosen{display: none}

border: 1px solid red;
}

.list-group-item {
  cursor: move;
}

.list-group-item i{
  cursor: pointer;
}
.list-group-item div{
  //cursor: pointer;
}

</style>
