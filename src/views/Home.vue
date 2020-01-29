<template>
  <div class="home">
    <div class="search">
      <div class="search-wrapper">
        <input
          type="text"
          placeholder="Search for a country..."
          @keyup.prevent="searchCountry"
          v-model="search"
        />
      </div>
    </div>
    <div class="content">
      <div
        class="country_card"
        v-for="country in countries"
        :key="country.name"
        @click="goToPage(`${country.name}`)"
      >
        <div class="country_flag">
          <img v-bind:src="country.flag" alt="Flag" />
        </div>
        <div class="country_info">
          <h3>{{country.name}}</h3>
          <div>
            <h5>
              <span>Population:</span>
              {{country.population.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}}
            </h5>
            <h5>
              <span>Region:</span>
              {{country.region}}
            </h5>
            <h5>
              <span>Capital:</span>
              {{country.capital}}
            </h5>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      countries: null,
      search: null,
      filter: null,
      options: ["Africa", "Americas", "Asia", "Europe", "Oceania"]
    };
  },
  methods: {
    goToPage(param) {
      this.$router.push("/country/" + param);
    },
    searchCountry() {
      axios
        .get(`https://restcountries.eu/rest/v2/name/${this.search}`)
        .then(res => {
          this.countries = res.data;
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  beforeCreate() {
    axios.get("https://restcountries.eu/rest/v2/all").then(response => {
      this.loading = false;
      this.countries = response.data;
    });
  }
};
</script>

<style lang="scss" scoped>
.content {
  padding: 20px 50px 20px 50px;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  grid-gap: 75px;
}

.country_card {
  border-radius: 10px;
  background-color: #fff;
  box-shadow: 1px 4px 6px 1px rgba(0, 0, 0, 0.1);
}

.country_flag {
  img {
    width: 100%;
    height: auto;
  }
}

.country_info {
  padding: 20px 10px 20px 20px;
  h5 {
    font-weight: 500;
    font-size: 14px;
    margin: 0;
    &:last-child {
      margin-bottom: 30px;
    }
  }
  span {
    font-weight: 800;
  }
}

.search {
  padding: 20px 50px 20px 50px;
}

.search-wrapper {
  box-shadow: 1px 4px 6px 1px rgba(0, 0, 0, 0.1);
  border-radius: 5px;
  width: 30%;

  input {
    border: none;
    padding: 16px;
    width: 100%;
    margin-left: 8px;
    background: transparent;
    outline: none;
    font-size: 14px;
  }
}
</style>
