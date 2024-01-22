<template>
<HeaderComponent />
<h1>Hello, Welcome On Update Restuarant Page</h1>
<form class="add-form">
    <label for="updateName">Name:</label>
    <input type="text" id="updateName" placeholder="Enter Name" v-model="restuarant.name" name="name" />

    <label for="updateAddress">Address:</label>
    <input type="text" id="updateAddress" placeholder="Enter Address" v-model="restuarant.address" name="address" />

    <label for="updateContact">Contact:</label>
    <input type="text" id="updateContact" placeholder="Enter Contact" v-model="restuarant.contact" name="contact" />

    <button type="button" @click="updateRestuarant">Update Restaurant</button>
</form>
</template>

<script>
import HeaderComponent from './Header.vue'
import axios from 'axios';
export default {
    name: "UpdateComponent",
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
        async UpdateRestuarant() {
            const result = await axios.put("http://localhost:3000/restuarant/" + this.$route.params.id, {
                name: this.restuarant.name,
                address: this.restuarant.address,
                contact: this.restuarant.contact
            });
            if (result.status == 200) {
                this.$router.push({
                    name: 'Home'
                });
            }
        }
    },
    async mounted() {
        let user = localStorage.getItem('user-info');
        if (!user) {
            this.$router.push({
                name: 'SignUp'
            })
        }

        const result = await axios.get("http://localhost:3000/restuarant?id=" + this.$route.params.id)
        this.restuarant = result.data[0]
    }
}
</script>
