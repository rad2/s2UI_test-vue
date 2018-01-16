<template>

  <div id="app">
    <div id="al_detail">
      <AccessLevel-List @showALname='listData' :updatedAlinfo='alObjdata'/>
    </div>
    <div id="al_frm"> 
       <Access-Levels :sendAlinfo='ObjData' @sendALObj='receivedAlObj' />
     </div>  
  </div>
</template>

<script>

// eslint-disable-next-line
/* eslint-disable */
import AccessLevels from './components/AccesslevelAdd'
import AccessLevelList from './components/AccesslevelList'
export default {
  name: 'app',
  data(){
     return{
       ObjData:'',
       alObjdata:''
     }
  },
  methods:{
       listData(data){
        this.ObjData = data;
       // console.log(this.ObjData);
      
       },
       receivedAlObj(data){
         this. alObjdata = data;

       }
  },
  components: {
   AccessLevels,
   AccessLevelList
  },
 
}
</script>

<style scoped>
#app {
    width:850px;
    margin-top: 60px;
    margin-left:20px;
    }
#al_detail{
   width:550px;
   float:left;
}
#al_frm{
  width:500px;
  margin-left:700px;
  
}
</style>
