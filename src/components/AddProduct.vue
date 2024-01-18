<template>
<form >
  <label >Product name</label>
  <input type="text" name="product" v-model="title"/>
  <br/>

  <label >Price</label>
  <input type="text" name="price" v-model="price"/>
  <br/>
  <label >Description</label>
  <input type="text" name="description" v-model="description"/>
  <br/>
  <label>Brand</label>
  <input type="text" name="brand" v-model="brand"/>
  <br/>
  <button v-on:click="addUser()">Add User</button>
</form>
</template>
<script>
import axios from 'axios'
export default{
  name:'AddProduct',
  data(){
    return{
      title:'',
      price:'',
      description:'',
      brand:'',
    }
  },
  methods:{
    async addUser(){
      let result = await axios.post("http://mail.zaxtom.com:3000/products",{
        title:this.title,
        price:this.price,
        description:this.description,
        brand:this.brand,
      });
      console.log(result.data);
    },

  }
}

</script>
<style scoped>
button{
  margin: 10px;
  padding: 10px;
  background-color: #4CAF50;
  color: white;
  border-radius: 5px;
  border: none;
}
</style>