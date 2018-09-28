<template>
  <div class="main">   
    <h1 v-if="id !== null">
	  Edit
	</h1>
	<h1 v-else>
	  Add
	</h1>
    <input type="text" ref="firstName" v-model="firstName" placeholder="firstName">
    <input type="text" ref="surname" v-model="surname" placeholder="surname">
    <input type="number" ref="phone" v-model="phone" placeholder="phone">
    <input type="email" ref="email" v-model="email" placeholder="email">
    <button @click="aditItem()" v-if="id !== null">
	  Edit
	</button>
	<button @click="addItem()" v-else>
	  Add
	</button>
    <router-link to="/">Main</router-link>
  </div>
</template>

<script>
export default {
  name: 'EditPage',
  data () {
    return {
      firstName: '',
	  surname: '',
	  phone: '',
	  email: '',
	  id: null
	}
  },
  mounted () {
  	const params = this.$route.params;
  	if (params.length > 0) {
  		this.firstName = params[0];
  		this.surname = params[1];
  		this.phone = +params[2];
  		this.email = params[3];
  		this.id = params[4];
  	} 

  }, 
  methods: {
    addItem () {
    	if (
			this.$refs.firstName.value === "" ||
			this.$refs.surname.value === "" ||
			this.$refs.phone.value === "" ||
			this.$refs.email.value === ""
    		) {
    		alert("error")
    	} else {
      let data = {};
      data.name = this.$refs.firstName.value;
      data.surname = this.$refs.surname.value;
      data.phone = this.$refs.phone.value;
      data.email = this.$refs.email.value;

      const items = JSON.parse(this.$localStorage.get('items')) || [];
      
      items.push(data);
      this.$localStorage.set('items', JSON.stringify(items));
      this.$router.push({ path: '/' })
  }
    },
    aditItem () {
    	if (
			this.$refs.firstName.value === "" ||
			this.$refs.surname.value === "" ||
			this.$refs.phone.value === "" ||
			this.$refs.email.value === ""
    		) {
    		alert("error")
    	} else {
    	console.log("obj");
    	const items = JSON.parse(this.$localStorage.get('items'));
    	let data = {};
		data.name = this.$refs.firstName.value;
		data.surname = this.$refs.surname.value;
		data.phone = this.$refs.phone.value;
		data.email = this.$refs.email.value;
		items.splice(this.id, 1, data);
		this.$localStorage.set('items', JSON.stringify(items));
	  	this.$router.push({ path: '/' })
	  }
    }
  }
}
</script>