<template>
  <div id="app">
    <WeatherContent :city="city" v-if="city !== ''" />
    <WeatherContent v-if="city === ''">
      <p>Location data loading...</p>
    </WeatherContent>
    <Search @getCity="setCity" />
  </div>
</template>

<script>
import WeatherContent from './components/WeatherContent.vue'
import Search from './components/Search.vue'

export default {
  name: 'app',
  data() {
    return {
      city: ''
    }
  },
  components: {
    WeatherContent,
    Search
  },
  methods: {
    getGeoData() {
      if ('geolocation' in navigator) {
        this.getDataWithGeo();
      } else {
        this.getDataWithIP();
      }
    },
    getDataWithGeo() {
      navigator.geolocation.getCurrentPosition(pos => {
        this.$http.get('https://maps.googleapis.com/maps/api/geocode/json', {
          params: {
            latlng: pos.coords.latitude + ',' + pos.coords.longitude,
            sensor: true,
            language: 'en'
          }
        }).then(response => {
            if (response.body.results) {
              response.body.results.some((el) => {
                if (el.types.includes('locality')) {
                  this.setCity(el.address_components[0].long_name);
                  return true;
                }
              });
            }
          })
          .catch(err => {
            console.error(err);
          });
      }, () => {
        this.getDataWithIP();
      });
    },
    getDataWithIP() {
      this.$http.get('https://ipapi.co/json').then(response => {
        if (response.body && response.body.city !== '') {
          this.setCity(response.body.city);
        }
      }).catch(err => {
        console.error(err);
      });
    },
    setCity(city) {
      this.city = city;
    }
  },
  beforeMount() {
    if (!this.city)
      this.getGeoData();
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
}

body {
  margin: 0;
  padding: 0;
}
</style>
