<template>
  <div class="hello">
    <h1>Header Table</h1>
    <router-link to="/edit">
      <button>
        Add user
      </button>
    </router-link>
    <input type="file"  
    multiple
    @change="filesChange"
    >
   <table id="centerTable">
     <thead>
       <tr>
         <th>name</th>
         <th>surname</th>
         <th>phone</th>
         <th>email</th>
       </tr>
     </thead>
     <tbody v-for="(item, index) in items">
       <tr>
          <td>{{ item.name }}</td>
          <td>{{ item.surname }}</td>
          <td>{{ item.phone }}</td>
          <td>{{ item.email }}</td> 
          <td>
            <button @click="aditItem(item.name,item.surname, item.phone, item.email, index)" 
            class="editItem">
              edit
            </button>
          </td>
          <td><button @click="deleteItem(index)" class="deleteItem">delete</button></td>
       </tr>    
     </tbody>
   </table>
  </div>
</template>

<script>
export default {
  name: 'MainPage',
  data () {
    return {
      text: "test",
      items: null
    }
  },
   mounted () {
    const items = JSON.parse(this.$localStorage.get('items'))
    if (items) {
      this.items = items
    }
  },
  methods: {
    aditItem (...params) {
      this.$router.push({ name: 'EditPage', params: params })
    },
    deleteItem (id) {
      console.log(id);
      this.items.splice(id, 1);

       this.$localStorage.set('items', JSON.stringify(this.items));
    },
    filesChange (e) {
      console.log("file",e.target.files[0]);
      // this.$emit('input', e.target.files[0]);
      console.log("this.items1",this.items);
      var reader = new FileReader();
      
      reader.onload = (e) => {
            console.log("onload FIRED", e)
              let jsonResult = e.target.result;
              console.log("jsonResult",JSON.parse(jsonResult).data);
              let data = JSON.parse(jsonResult)
              this.items = data.data;
              console.log("this.items",this.items);
              this.$localStorage.set('items', JSON.stringify(this.items));
            }
            reader.readAsText(e.target.files[0])
            
     }  
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
},
#centerTable {
  margin: 0 auto;
},
thead {
  font-size: 15
},
tbody {
  font-size: 13
},
td {
padding: 10px;
  }
</style>
