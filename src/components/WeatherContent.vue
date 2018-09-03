<template>
  <div class="container" :style="style">
        <div class="content">
            <WeatherData :data="weatherData" :city="city" v-if="weatherData && city" />
            <slot></slot>
        </div>
  </div>
</template>

<script>
import WeatherData from './WeatherData.vue'
import keys from '../config/keys'

export default {
  name: 'WeatherContent',
  components: {
      WeatherData
  },
  props: {
      city: String
  },
  data() {
      return {
          url: '',
          weatherData: {}
      }
  },
  watch: {
      city() {
          if (this.city) {
              this.getWeather();
          }
      }
  },
  computed: {
      style() {
          return 'background-image: url(' + this.url + ')';
      }
  },
  methods: {
      getWeather() {
          this.$http.get('https://api.openweathermap.org/data/2.5/weather', {
              params: {
                  q: this.city,
                  APPID: keys.openweathermap.key,
                  units: 'metric'
              }
          }).then(res => {
              this.weatherData = {
                  description: res.body.weather[0].description,
                  icon: res.body.weather[0].icon,
                  temp: res.body.main.temp
              }
              this.loadCityImage(this.weatherData.description);
          }).catch(err => {
              console.error(err);
          });
      },
      loadCityImage(weather = '') {
          this.$http.get('https://api.flickr.com/services/rest', 
          {
              params: {
                  method: 'flickr.photos.search',
                  api_key: keys.flickr.key,
                  text: this.city + ' ' + weather,
                  safe_search: 1,
                  content_type: 1,
                  media: 'photos',
                  per_page: 50,
                  format: 'json',
                  nojsoncallback: 1,
                  extras: 'url_h',
                  sort: 'relevance'
                }
          }).then(res => {
              const photos_with_url = res.body.photos.photo.filter(obj => {
                  return typeof obj.url_h !== 'undefined';
              });
              if (photos_with_url.length > 0) {
                    const index = Math.floor(Math.random() * Math.floor(photos_with_url.length - 1));
                    this.url = photos_with_url[index].url_h;
              } else {
                    this.loadCityImage(); // load without weather search query
              }
          }).catch(err => {
              console.error(err);
          })
      }
  },
  beforeMount() {
      if (this.city) {
          this.getWeather();
      }
  }
}
</script>

<style scoped>
    .container {
        height: 100vmin;
        width: 100%;
        background-size: cover;
        background-position: center center;
    }
    .container:after {
        content: " ";
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.75);
        display: block;
        z-index: 0;
    }
    .content {
        position: relative;
        z-index: 1;
        top: 50%;
        margin: auto;
        transform: translateY(-50%);
        max-width: 100%;
        min-width: 350px;
        width: 20%;
    }
</style>