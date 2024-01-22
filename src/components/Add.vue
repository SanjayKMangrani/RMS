<template>
<HeaderComponent />
<h1>Hello, Welcome On Add Restuarant Page</h1>
<form class="add-form">
    <label for="name">Name:</label>
    <input type="text" id="name" placeholder="Enter Name" v-model="restuarant.name" name="name" />

    <label for="address">Address:</label>
    <input type="text" id="address" placeholder="Enter Address" v-model="restuarant.address" name="address" />

    <label for="contact">Contact:</label>
    <input type="text" id="contact" placeholder="Enter Contact" v-model="restuarant.contact" name="contact" />

    <button type="button" @click="addRestuarant">Add New Restaurant</button>
</form>
</template>

<script>
import axios from 'axios';
import HeaderComponent from './Header.vue'
export default {
    name: "AddComponent",
    components: {
        HeaderComponent
    },
    data() {
        return {
            restuarant: {
                name: '',
                address: '',
                contact: ''
            }
        }
    },
    methods: {
        async addRestuarant() {
            const result = await axios.post("http://localhost:3000/restuarant", {
                name: this.restuarant.name,
                address: this.restuarant.address,
                contact: this.restuarant.contact
            });
            if (result.status == 201) {
                this.$router.push({
                    name: 'Home'
                });
            }
        }
    },
    mounted() {
        let user = localStorage.getItem('user-info');
        if (!user) {
            this.$router.push({
                name: 'SignUp'
            })
        }
    }
}
</script>
