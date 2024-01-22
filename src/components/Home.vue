<template>
<HeaderComponent />
<h1>Hello, {{ name }} Welcome On Home Page</h1>
<div class="centered-container">
    <div class="restaurant-list">
        <div v-for="restaurant in restuarants" :key="restaurant.id" class="restaurant-card">
            <h3>{{ restaurant.name }}</h3>
            <p>{{ restaurant.address }}</p>
            <p>Contact: {{ restaurant.contact }}</p>
            <div class="actions">
                <router-link :to="'/update/' + restaurant.id" class="action-link">Update</router-link>
                <button @click="deleteRestaurant(restaurant.id)" class="action-button">Delete</button>
            </div>
        </div>
    </div>
</div>
</template>

<script>
import HeaderComponent from './Header.vue'
import axios from 'axios';
export default {
    name: "HomeView",
    data() {
        return {
            name: '',
            restuarants: [],
        }
    },
    components: {
        HeaderComponent
    },
    methods: {
        async deleteRestuarant(id) {
            let result = await axios.delete("http://localhost:3000/restuarant?id=" + id)
            if (result.status === 200) {
                this.loadData()
            }

        },
        async loadData() {
            let user = localStorage.getItem('user-info');
            this.name = JSON.parse(user).name
            if (!user) {
                this.$router.push({
                    name: 'SignUp'
                })
            }
            let result = await axios.get("http://localhost:3000/restuarant")
            this.restuarants = result.data
        }
    },
    mounted() {
        this.loadData()
    }
}
</script>

<style>
.centered-container {
    display: flex;
    justify-content: center;
}

.restaurant-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;
    /* Align rows to the left */
}

.restaurant-card {
    background-color: #ffeed9;
    max-width: 300px;
    width: calc(33.33% - 20px);
    /* Set width for 3 cards per row with 20px margin between cards */
    margin: 10px;
    /* Margin between cards */
    border: 1px solid #dddddd;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    box-sizing: border-box;
}

.restaurant-card h3 {
    margin-bottom: 8px;
}

.actions {
    margin-top: 16px;
    display: flex;
    justify-content: space-between;
}

.action-link,
.action-button {
    cursor: pointer;
    color: #007BFF;
    text-decoration: underline;
}

.action-button {
    background-color: #DC3545;
    color: white;
    border: none;
    padding: 6px 12px;
    border-radius: 4px;
}
</style>
