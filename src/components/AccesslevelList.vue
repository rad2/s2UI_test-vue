<<template>
  <div>
     
            <p>{{lbl}}</p>
                       
            <table class="table">
                <thead>
                <tr style="background:grey">
                    <th colspan="3">
                        <span class="glyphicon glyphicon-plus special"></span> 
                       <input type="search" placeholder="Spotlight Search" class="form-control" name="search" v-model="search">
                    </th>    
                 </tr>       
                <tr>
                    <th v-for="c in columns">{{c}}</th>
                </tr>
                </thead>
                <tbody>
                <tr v-for='a in almap' :key="a.id"  @click="list(a)">
                   <td class="row">{{a.name}}</td>
                    <td class="row">{{a.rdtypename}}</td>
                    <td class="row">{{a.rdname}}</td>
                </tr>
                </tbody>
            </table>      
  </div>

</template>


<<script>
// eslint-disable-next-line
/* eslint-disable */
import axios from 'axios';
import lodash from 'lodash';
export default {
  name: 'AccessLevel-List',

  data() {
        
      return{
          lbl:'AccessLevels',
          columns:['Name','Redaer Type','Reader(s)'],
          al:[],
          rd:[],
          rdtype:[],
          search:''
          
      }
  },
methods:{
      
   alfunc(){
            axios.get('src/assets/data/accesslevels.json')
                 .then((res) => {this.al = res.data;})
                 .catch(error => console.error(error));
            
            axios.get('src/assets/data/readers.json')
                 .then((res) =>{ this.rd = res.data;})
                 .catch(error => console.error(error));

            axios.get('src/assets/data/readertype.json')
                 .then((res) =>{ this.rdtype = res.data;})
                 .catch(error =>console.error(error));
   }, 
   list(a){
       //console.log(a);
       this.$emit("showALname", a)
   }

},
mounted(){
  this.alfunc();
  
},
computed:{
    
   almap: function(){
       var al_map=[];
      var als =[];
      var vm = this;
       this.al.map(function(alObj){
        vm.rd.find(function(rdObj){   
           if( alObj.readerId === rdObj.id){
               alObj.rdname = rdObj.name;
               alObj.rdtypeId = rdObj.typeId;
             
              al_map.push(alObj);
          }
        })   
           //console.log(al_map);
      })

       al_map.forEach(function(item){
        vm.rdtype.find(function(rdtypeObj){
           if(item.rdtypeId === rdtypeObj.id){
              item.rdtypename= rdtypeObj.name;
             
              als.push(item);
           }
       }) 
       
    })   
     return als.filter(function(item){
         var searchRegex = new RegExp(vm.search,'i')
         return searchRegex.test(item.name) ||
                 searchRegex.test(item.rdname) ||
                 searchRegex.test(item.rdtypename) 
        })
   }  
}
}
</script>
<<style scoped>
  .special {
  position: relative;
  top: 0;
  left:-5px;
}
.row{
    cursor: pointer;
}
</style>

