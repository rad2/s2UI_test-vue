<template>
  <div>
   <div id="readers">
    
 
      <select  v-model="readerId" @change="getReaderId" class="form-control input-sm">
       <option value=0 selected>All Reader Groups</option>
       <option v-for="r in readers" :key="r.id" :value="r.id" >{{r.name}}</option>
      </select>  
		</div>	
  </div>
</template>

<script>
// eslint-disable-next-line
/* eslint-disable */
import axios from 'axios';
export default {
	name: 'Readers',
	props:['readerId'],
  data () {
    return {
			lbl: 'Reader(s)',
			selected:0,
      readers:[]

    }
  },
  methods:{
      getReaderId(){
				this.$emit('setReaderId',this.selected);
			}
	},
	created(){
			axios.get('src/assets/data/readers.json')
			        .then(res => {
								this.readers = res.data;
							})
							.catch(error => console.error(error));
	},
	computed:{
		initrdId:function(){
       return this.readerId=0;
		}
		 
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
label {
    float: left;
    padding: 10px 10px 14px 0;
    width: 175px;
    margin-top: 10px;
    clear: left;
  }
	input[select]{
    float:right;
	}
</style>
