<<template>
   <div>
      
            <p>{{title}}</p>
                <form method="POST" @click.prevent="save" class="form-horizontal">
                  
                        <label class="control-label " for="al_name">{{lbl_name}}:</label>
                       
                        <input type="text" id="alname"  class="form-control input-sm" v-model="sendAlinfo.name" >           
                 
                        <label class="control-label " for="al_desc">{{lbl_description}}:</label>
                             
                        <textarea row="100" col="140" class="form-control" id="al_desc" name="aldesc" v-model.lazy="sendAlinfo.description"></textarea>
                        <label class="control-label " for="rd">Reader(s):</label>
                         <p id="rd">
                             <Readers @setReaderId="r_id=$event" :readername='sendAlinfo.rdname' :readerId ='sendAlinfo.readerId'/>
                          </p><br/>
                         <div id="btn">
                           <button id="save" @click="update()">Save</button>&nbsp;<button id="cancel">Cancel</button>
                         </div>
                </form>
   </div>
</template>


<script>
// eslint-disable-next-line
/* eslint-disable */
import Readers from './Readers';
import axios from 'axios';
export default {
  name: 'Access-Levels',
  props:['sendAlinfo'],
  data(){
      return{
          title:'Access Name',
          lbl_name: 'Name',
          lbl_description: 'Description',
          btn_save: 'Save',
          btn_cancel: 'Cancel',
          r_id:'',
          //al_id:this.sendAlinfo.al_id,
          accesslevels:[]
      }
     },
    components:{
        Readers
    },
    methods: {
        incrementId: function(){
           let lastEl = this.accesslevels[this.accesslevels.length -1];
          return lastEl.id++;
        },
        update: function(){
            var vm = this;
          return this.accesslevels.find(function(alObj){
              if (alObj.id === vm.sendAlinfo.id){
                  alObj.description = vm.sendAlinfo.description;
                  alObj.name = vm.sendAlinfo.name;
                  alObj.readerId = vm.r_id;

                  this.$emit('sendReaderId', vm.r_id);
              } 
          })
          
        },
        save: function(){
            let al_Obj= {
                    id:this.incrementId(),
                    name:this.sendAlinfo.alname,
                    readerId:this.r_id,
                    description:this.sendAlinfo.aldesc
                }
            this.accesslevels.push(al_Obj); 
            
            //console.log(JSON.stringify(this.accesslevels));          
        },
        
    },
    mounted(){
       
    },
    created(){
        axios.get('src/assets/data/accesslevels.json')
                 .then((res) => {this.accesslevels = res.data;})
                 .catch(error => console.error(error));
    }
}
</script>

<style scoped>

  button{
      margin-top:20px;
      
  }
  #btn{
       float:left;
  }
  

</style>

