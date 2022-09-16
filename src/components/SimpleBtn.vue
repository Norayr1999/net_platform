<template>
  <div>
      <div class="dropdown">
        <button class="btn btn-light dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          Location <span class="badge badge-success rounded-pill" v-if="countSelected">{{ countSelected }}</span>
        </button>
        <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
          <div class="baseDropList">
            <span>Country</span>
            <input type="text" class="form-control searchField" placeholder="Type to search" v-model="search_country">
            <div class="scroll">
              <div class="form-group" v-for="(country,index) in filterCountries" :key="index">
              <div class="checkbox checkbox-success checkbox_header d-flex align-items-center">
                <input class="input_checkbox" type="checkbox" :value="country" :id="'checkbox_'+ country" v-model="selectedCountries[index]">
                <label class="checkbox_level" :for="'checkbox_'+ country">{{ country }}</label>
              </div>
              </div>
            </div>
            <div class="m-top-25">
              <span>Region</span>
              <input type="text" class="form-control searchField" placeholder="Type to search" v-model="search_region">
              <div class="scroll">
                <div class="checkbox checkbox-success checkbox_header d-flex align-items-center" v-for="(region, index) in filterRegions" :key="index">
                  <input class="input_checkbox" type="checkbox" :id="'checkbox_'+ region" :value="region" v-model="selectedRegions[index]">
                  <label class="checkbox_level" :for="'checkbox_'+ region">{{ region }}</label>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import {countries, regions} from '@/assets/list'

export default defineComponent({
  props: {
    msg: {
      type: String,
      default: ''
    },
  },
  data() {
    return {
      search_country: '',
      search_region: '',
      selectedCountries: [],
      selectedRegions: [],
      countries: countries,
      regions: regions
    }
  },
  computed: {
    countSelected () {
      return this.selectedCountries.filter(el => el).length + this.selectedRegions.filter(el => el).length
    },
    filterCountries() {
      return this.countries.filter((el: string) => (el.toLowerCase()).includes(this.search_country.toLowerCase()))
    },
    filterRegions() {
      return this.regions.filter((el: string) => (el.toLowerCase()).includes(this.search_region.toLowerCase()))
    }
  }
});
</script>

<style lang="scss" scoped>
$eight: -8px;
$grey: #b2b2b2;
$x25: 25px;

#dropdownMenuButton {
  position: relative;
  border-radius: $x25;
  border: 1px solid $grey;
  color: $grey
}
.dropdown-menu {
  border-radius: 20px;
}
.input_checkbox {
  width: 20px;
  height: 20px;
  margin: 3px;
}
.checkbox_level {
  font-size: 16px;
  margin-left: 10px;
}
.baseDropList{
  padding: 0 20px
}
.checkbox:hover {
  background: #79cf8a
}
.searchField {
    border-radius: $x25;
    left: 0;
    right: 0;
    margin: auto;
}
.badge {
  position: absolute;
  right: $eight;
  top: $eight;
}
.dropdown-toggle::after {
  display: none !important;
}
.scroll{
  margin-top: 10px;
  height: 200px;
  overflow-y: scroll;
}
.m-top-25 {
  margin-top: $x25;
}
</style>
