<template>
  <div class="weather">
    <div><b>{{cityData.name}} {{cityData.country}}</b></div>
    <div class="weather__temp">
      <v-icon class="weather__cloud" name="cloud-sun"></v-icon>
      <span class="weather__deg">{{ temp }}</span>
    </div>
    <div class="weather__desc">{{description}}</div>
    <div class="weather__pressure">
      <v-icon name="compass"></v-icon> <span> {{pressure}}</span>
      <v-icon name="location-arrow"></v-icon>  <span> {{winterSpeed}}</span>
    </div>
    <div class="weather__humidity">
      <span> <b>Humidity</b> {{humidity}} </span>
      <span> <b>Visibility</b> {{visibility}} </span>
    </div>
  </div>
</template>

<script>
import { requestsMixin } from "@/mixins/requestsMixin";

import 'vue-awesome/icons/flag'
import 'vue-awesome/icons'
import Icon from 'vue-awesome/components/Icon'

export default {
  name: "Weather",
  components: {
    'v-icon': Icon
  },
  mixins: [requestsMixin],

  data() {
    return {
      weather: {
        main: {},
        wind: {},
        weather: []
      },
      cityData: {}
    };
  },

  props: {
    city: String
  },

  computed: {
    temp() {
      return `${Math.round(this.weather.main.temp - 273.15)}°C`
    },
    pressure() {
      return `${this.weather.main.pressure} hPa`
    },
    winterSpeed() {
      return `${this.weather.wind.speed} km/s`
    },
    visibility() {
      return `${(this.weather.visibility / 1000).toFixed(1)} km`
    },
    humidity() {
      return `${this.weather.main.humidity}%`
    },
    description() {
      let arrW = this.weather.weather.map(w=> w.main)
      return `${arrW.join()}`
    }
  },
  async created() {
    const response = await this.searchForecast(this.city);
    this.weather = response.data.list[0];
    this.cityData = response.data.city;
  }
};
</script>

<style lang="scss">

.weather {
  font-size: 14px;
  margin-bottom: 32px;
  &:last-child {
    margin-bottom: 0;
  }
  & > div {
    margin-bottom: 16px;
  }
  &__cloud  {
    width: 100px;
    height: 80px;
  }
  &__deg {
    font-weight: bold;
    font-size: 40px;
    margin-left: 16px;
  }
  &__desc {
    margin-top: 24px;
  }
  &__pressure {
    & > span {
      margin-right: 24px;
    } 
  }
  &__humidity { 
    & > span { 
      margin-right: 16px;
    }
  }
}

</style>
