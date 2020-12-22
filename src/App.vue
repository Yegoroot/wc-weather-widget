<template>
  <div :class="weatherWrapClass">
    <v-icon :class="settingsIconClass" @click="onOpen" name="cog"></v-icon>
    <div :class="settingsClass" v-if="isOpen">
      <h3>Settings</h3>
      <div class="settings__mode" @click="onChangeMode">
        <v-icon v-if="mode==='light'" name="moon"></v-icon>
        <v-icon v-if="mode==='dark'" name="sun"></v-icon>
      </div>
      <div v-for="c in cities" :key="c.name" class="settings__city">
        <v-icon  @click="onOpen" name="bars"></v-icon>
        <span> {{c.name}} </span>
        <v-icon @click="()=>onDelete(c.name)" name="trash"></v-icon>
      </div>
      <div class="settings__input">
        <input type="text" v-model="value">
        <v-icon @click="onAdd" name="plus-square"></v-icon>
      </div>
    </div>
    <Weather v-for="c in cities" :key="c.name" :city="c.name" />
  </div>
</template>

<script>
import Weather from '@/components/Weather.vue?shadow';
import 'vue-awesome/icons/flag'
import 'vue-awesome/icons'
import Icon from 'vue-awesome/components/Icon'

export default {
  name: 'Home',
  components: {
    Weather,
    'v-icon': Icon
  },
  data() {
    return {
      mode: 'light',
      isOpen: false,
      cities: [
        {
          name: 'moscow'
        }
      ],
      value: ''
    }
  },
  methods: {
    onOpen() {
      this.isOpen = !this.isOpen
    },
    onDelete(cityName) {
      this.cities = this.cities.filter(city=> city.name !== cityName)
    },
    onChangeMode() {
      this.mode === 'dark' ? this.mode = 'light' : this.mode = 'dark'
    },
    onAdd() {
      this.cities.push({
        name: this.value
      })
      this.value = ""
    }
  },
  computed: {
    settingsIconClass() {
      return {
        'weather-wrap__icon': true,
        'dark': this.mode === 'dark',
        'light': this.mode === 'light'
      }
    },
    settingsClass() {
      return {
        "settings": true, 
        'dark': this.mode === 'dark',
        'light': this.mode === 'light',
      }
    },
    weatherWrapClass() {
      return {
        'weather-wrap' : true, 
        'dark': this.mode === 'dark',
        'light': this.mode === 'light',
      }
    } 
  }
};
</script>

<style lang="scss" >
.weather-wrap {
  font-family: sans-serif;
  position: relative;
  width: 250px;
  padding: 10px;
  &__icon {
    position: absolute;
    top: 20px;
    left: 240px;
    height: 20px;
    width: 20px;
    cursor: pointer;
    z-index: 2;
  }
}
 .settings {
    position: absolute;
    background: #ffffff;
    box-sizing: border-box;
    width: 100%;
    padding: 20px;
    box-shadow: rgba(0, 0, 0, 0.2) 0px 3px 3px -2px, rgba(0, 0, 0, 0.14) 0px 3px 4px 0px, rgba(0, 0, 0, 0.12) 0px 1px 8px 0px;
    left: 8px;
    top: 8px;
    z-index: 1;
    padding-bottom: 40px;
    h3 { 
      margin-top: 0px;
      font-size: 25px;
    }
    &__mode {
      position: absolute;
      top: 12px;
      right: 55px;
      & svg {
        height: 20px;
        width: 20px;
      }
    }
    &__city {
      cursor: pointer;
      display: flex;
      justify-content: space-between ;
      margin-bottom: 16px;
      & span {
        width: 180px
      }
    }
    &__input {
      & input {
        flex: 1;
      }

      & svg {
        cursor: pointer;
        width: 22px;
        height: 22px; 
        margin-left: 10px;
      }
      justify-content: space-between;
      display: flex;
    }
  }

  .dark {
    background: #3b3b40;
    color: #ffffff;
  }
  .light { 
    background: white;
    color: #3b3b40
  }

</style>