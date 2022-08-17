<template>
  <div class="col-7">
    <img
      v-bind:src="`https://flagpedia.net/data/flags/w702/${alpha2Code.toLowerCase()}.png`"
      alt="country flag"
      style="width: 300px"
    />
    <h1>{{ name }}</h1>
    <table class="table">
      <thead></thead>
      <tbody>
        <tr>
          <td style="width: 30%">Capital</td>
          <td>{{ capital }}</td>
        </tr>
        <tr>
          <td>Area</td>
          <td>{{ area }} km <sup>2</sup></td>
        </tr>
        <tr>
          <td>Borders</td>
          <td>
            <ul>
              <li v-for="(border, index) in borders" :key="index">
                <router-link :to="`/list/${border}`">{{ border }}</router-link>
              </li>
            </ul>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "CountryDetails",

  data() {
    return {
      name: "",
      capital: "",
      alpha3code: "",
      alpha2code: "",
      area: "",
      borders: [],

      countryInfo: {},
    };
  },

  methods: {
    async getCountryByAlphaCode() {
      console.log("estoy dentro de getCountryByAlphaCode");
      this.alpha3code = this.$route.params.alpha3code;
      const response = await fetch(
        `https://ih-countries-api.herokuapp.com/countries/${alpha3Code}`
      );
      

      const finalResponse = await response.json();

      console.log(finalResponse);

      this.name = finalResponse.name.common;
      this.capital = finalResponse.capital[0];
      this.area = finalResponse.area;
      this.borders = finalResponse.borders;
      this.alpha2code = finalResponse.alpha2code;

      this.countryInfo = finalResponse;
    },
  },

  mounted() {
    this.getCountryByAlphaCode();
  },

  computed: {
    countryCode() {
      return this.$route.params.alpha3Code;
    },
  },

  watch: {
    countryCode(newCountryCode) {
      this.getCountryByAlphaCode();
    },
  },
};
</script>

<style></style>
