<template>
  <div>
    <div class="back-button-wrapper">
      <router-link to="/">
        <button class="back-button">
          <span>&larr;</span> Back
        </button>
      </router-link>
    </div>
    <div class="country" v-for="item in country" :key="item.name">
      <div class="country-flag">
        <img :src="item.flag" :alt="item.name" />
      </div>

      <div class="country-content">
        <div>
          <h1>{{ item.name }}</h1>
        </div>

        <div class="country-details">
          <h4>
            Native Name:
            <span class="light-text">{{ item.nativeName }}</span>
          </h4>

          <h4>
            Population:
            <span
              class="light-text"
            >{{ item.population.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}}</span>
          </h4>

          <h4>
            Region:
            <span class="light-text">{{ item.region }}</span>
          </h4>

          <h4>
            Sub Region:
            <span class="light-text">{{ item.subregion }}</span>
          </h4>

          <h4>
            Capital:
            <span class="light-text">{{ item.capital }}</span>
          </h4>

          <h4>
            Top Level Domain:
            <span class="light-text">{{ item.topLevelDomain.toString() }}</span>
          </h4>
          <h4>
            Currency:
            <span class="light-text">{{ item.currencies[0].name }}</span>
          </h4>

          <h4>
            Languages:
            <span class="light-text">{{ languages }}</span>
          </h4>
        </div>

        <div class="country-details">
          <h4>Border Countries:</h4>
          <div class="border-country-wrapper">
            <div class="border" v-for="(borders, index) in borderCountries" :key="index">
              <span class="light-text">{{borders}}</span>
            </div>
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
      country: []
    };
  },
  mounted() {
    let countryName = this.$route.params.query;
    axios
      .get(`https://restcountries.eu/rest/v2/name/${countryName}?fullText=true`)
      .then(res => {
        this.country = res.data;
      });
  },
  computed: {
    languages() {
      let langArr = this.country[0].languages;
      let emptyArr = [];
      langArr.forEach(data => {
        emptyArr.push(data.name);
      });
      return emptyArr.join(", ");
    },
    borderCountries() {
      let borderCountry = this.country[0].borders;
      return borderCountry;
    }
  }
};
</script>

<style lang="scss">
.border-country-wrapper {
  display: flex;
  width: 100%;
}

.country {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 30px;
}

.country,
.country-content,
.country-details {
  margin: 24px 0;
}

.country-flag {
  width: 100%;
  img {
    max-width: 100%;
    height: auto;
    padding-left: 50px;
  }
}

.light-text {
  font-weight: 300;
}

.back-button-wrapper {
  padding: 25px 50px 25px 50px;
  .back-button {
    display: flex;
    align-items: center;
    padding: 1em;
    box-shadow: 0 2px 2px -2px rgba(0, 0, 0, 0.2);
    border-radius: 5px;
    border: none;
    cursor: pointer;
    outline: none;
    font-size: 15px;
    background-color: #fff;
    span {
      padding-right: 5px;
    }
  }
  a {
    text-decoration: none;
    color: inherit;
  }
}

.border-country-wrapper {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  .border {
    font-weight: bold;
    margin: 8px 0;
    padding: 5px;
    text-align: center;
    border-radius: 5px;
    box-shadow: 0 2px 2px -2px rgba(0, 0, 0, 0.2);
    transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
    &:not(:first-child) {
      margin-left: 8px;
    }
  }
}

.border:not(:first-child) {
  padding-left: 5px;
}
</style>