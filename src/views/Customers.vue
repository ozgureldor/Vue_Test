<template>
<div class="home">
    <div class="col-md-6 centeralign">
        <h1>Public Educations </h1>
        <p>This Page displays a list oof available Educations  1 </p>
         <div class="card centeralign addmargin" style="width: 18rem;" v-for="customer in customerlist" :key="customer.id">
            <div class="card-body" v-on:click="setSelectedCustomer(customer.name)">
                <h5 class="card-title">{{customer.title}}</h5>
                <p class="card-text">{{customer.price}}</p>
           

                 
            </div>
        </div>
    </div>
   <city-picker></city-picker>
</div>  
</template>
<style scoped>

.addmargin {
    margin-top: 10px;
    margin-bottom: 10px;
}

.vue-logo-back {
    background-color: black;
}

</style>

<script>

// @ is an alias to /src
import Display from '@/components/Display.vue'
import axios from 'axios'
import CityPicker from '../components/CityPicker.vue';

export default {
    name: 'customers',
    mounted() {
        axios({
            method: "GET",
            "url": "http://45.194.27.15:9001/api/education/educations/1"
        }).then(response => {
            this.customerlist = response.data;
        }, error => {
            // eslint-disable-next-line
            console.error(error);
        });
    },
    data() {
        return {
            customerlist: [],
            selectedCustomer: ""
        }
    },
    components: {
        Display,
        CityPicker
    },
    methods: {
        setSelectedCustomer: function(name) {
            this.selectedCustomer = name;
        },
        goToDetailsPage: function(id) {
            this.$router.push("/customerdetails/"+id);
        }
    }
}

</script>
