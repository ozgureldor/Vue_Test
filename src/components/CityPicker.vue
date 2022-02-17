<template>
  <div class="cascading-dropdown">
    <div class="dropdown">
      <H2>Build a reusable dropdown vue component</H2>
      <span>Country:</span>
      <select v-model="selectedCountry" @change="onChangeCountry">
        <option value="">Select a Country</option>
        <option
          v-for="(country, index) in listCountry"
          :value="country.id"
          :key="index"
        >
          {{ country.name }}
        </option>
      </select>
    </div>
    <div class="dropdown">
      <span>State:</span>
      <select
        :disabled="listState.length == 0"
        v-model="selectedState"
        @change="onChangeState"
      >
        <option value="">Select a State</option>
        <option
          v-for="(state) in listState"
          :key="state.name"
          :value="state.id"
        >
          {{ state.name }}
        </option>
      </select>
    </div>
    <div class="dropdown">
      <span>State:</span>
      <select :disabled="listCities.length == 0" v-model="selectedCity">
        <option value="">Select a City</option>
        <option
          v-for="(city) in listCities"
          :key="city.id"
          :value="city.name"
        >
          {{ city.name }}
        </option>
      </select>
    </div>
    <p v-if="selectedCountry">Data Source - {{ this.host }}</p>
 
  </div>
</template>

<script>
import axios from "axios";

var host="http://sapi.dagtalah.com:9051";


export default {
 
  data() {
    return {
      listCountry: [],
      listState: [],
      listCities: [],
      selectedCountry: "",
      selectedState: "",
      selectedCity: "",
      authToken: "",
      host:""
    };
  },
  created() {
    this.loadCountry() ;
    this.host=host;
  },
  methods: {
    generateAccessToken() {
      axios
        .get("https://www.universal-tutorial.com/api/getaccesstoken", {
         
        })
        .then((res) => {
          this.authToken = res.data.auth_token;
          this.loadCountry();
        });
    },
    loadCountry() {
      axios
        .get(host+"/countries", {
          headers: {
            Authorization: `Bearer ${this.authToken}`,
            Accept: "application/json",
          },
        })
        .then((res) => {
          this.listCountry = res.data;
          console.log("Date Received from :"+ host);
        });
    },
    onChangeCountry() { 
      console.log(this.selectedCountry);
      axios
        .get(
          host+`/states?countryid=${this.selectedCountry}`,
          {
            
          }
        )
        .then((res) => {


          this.listState = res.data;
        });
    },
    onChangeState() {
      axios
        .get(
         host+ `/cities?state=${this.selectedState}`,
          {
            
          }
        )
        .then((res) => {
          this.listCities = res.data;
        });
    },
  },
};
</script>
