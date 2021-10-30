<template>
  <div v-if="!isLoading">
    <div class="container">
    <!-- Title -->
    <div class="topheadWrap">
      <div class="headingWrap">
        <h1>Covid 19 Tracker</h1>
        <p>This dashboard keeps track of COVID-19 cases and deaths globally.</p>
      </div>
    </div>
    <div class="global_section">
      <CardsIndex :globalData="globalData" />
    </div>

    <div class="select_picker_container">
      <SelectPickerContainer
        v-if="countries"
        :countryData="countries"
        @countryChanged="fetchCountryData"
        ref="select2Ref"
      />
      <span class="btnwrap">
      <JsonCSV
        :data="countryReport">
        Download
      </JsonCSV>
      </span>
    </div>
    
    <div class="chart_container">
       <transition name="slide-fade">
      <LineChart
        v-if="countryReport"
        :countryReport="countryReport"
        ref="chartRef"
      />
      </transition>
    </div>
    
  </div>
  </div>
  <div v-else>Loading</div>
</template>

<script>
import JsonCSV from 'vue-json-csv'
export default {
  components:{
    JsonCSV
  },
  data() {
    return {
      countries: [],
      globalData: {},
      isLoading: true,
      countryReport: [],
    };
  },
  methods: {
    async fetchGlobalStats() {
      let { data } = await this.$axios.get(
        "https://api.covid19api.com/summary"
      );
      return data;
    },
    async fetchCountryData(country) {
      const vim = this;
      var curDate = new Date();
      const curMonthDate =
        curDate.getFullYear() +
        "-" +
        curDate.getMonth() +
        "-" +
        curDate.getDate();
      curDate.setMonth(curDate.getMonth() - 3);
      const last6thMonth = curDate.getFullYear() + "-" + curDate.getMonth();
      const startDate = last6thMonth + "-01T00:00:00Z";
      const endDate = curMonthDate + "T00:00:00Z";
      await this.$axios
        .get(
          "https://api.covid19api.com/country/" +
            country +
            "?from=" +
            startDate +
            "&to=" +
            startDate
        )
        .then(function (res) {
          vim.countryReport = res.data;
          vim.$refs.chartRef.initChart(res.data);
        });
    },
    async getUserData() {
      const vim = this;
      await this.$axios.get("https://api.freegeoip.app/json/?apikey=27d088a0-365c-11ec-bd89-a3c9410a18ce").then((result) => {
        console.log(" result : ", result);
        const countryName = result.data.country_name;
        if (countryName) {
          const countryData = vim.countries.find(({ Country, CountryCode }) => {
            console.log(
              "country.Country.toLowerCase() == result.country_name.toLowerCase()"
            );
            console.log(Country);
            console.log("result.country_name");
            console.log(countryName);
            return Country.toLowerCase() == countryName.toLowerCase();
          });
          vim.$refs.select2Ref.selectCountry({
            text: countryData.Country,
            label: countryData.Country,
            value: countryData.CountryCode,
          });
          vim.fetchCountryData(countryData.Country);
        }
      });
    },
  },
  created() {
    const vim = this;
    this.fetchGlobalStats().then(function (res) {
      vim.isLoading = false;
      vim.globalData = res.Global;
      vim.countries = res.Countries;
      vim.getUserData();
    });
  },
};
</script>
<style lang="css">

</style>