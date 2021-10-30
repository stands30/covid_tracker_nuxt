<template>
  <div v-if="loaded" class="wrapdropdown">
    <vueSelect2
      class="vue-select2"
      name="select2"
      :options="countryOptions"
      :model.sync="selectedCountry"
      :value="selectedCountry"
      :searchable="true"
      @input="triggerCountryChanged"
    >
    </vueSelect2>
  </div>
</template>

<script>
import vueSelect2 from "vue-select";
import "vue-select/dist/vue-select.css";

export default {
  props: ["countryData"],
  components: {
    vueSelect2,
  },
  data() {
    return {
      countryOptions: [],
      selectedCountry: "",
      loaded: false,
    };
  },
  methods: {
      triggerCountryChanged: function (newVal) {
        this.selectedCountry = newVal;
        this.$emit("countryChanged", newVal.value);
      },
      selectCountry: function(selectedVal){
          console.log('setSelect2 ', selectedVal);
          this.selectedCountry = selectedVal;
      }
  },
  mounted() {
    const vim = this;
    vim.countryData.map(({ Country, CountryCode }) => {
      vim.countryOptions.push({
        text: Country,
        label: Country,
        value: CountryCode,
      });
    });
    vim.loaded = true;
  },
};
</script>

<style>
</style>