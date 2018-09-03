<template>
    <div class="weatherdata">
        <h1>{{ city }}</h1>
        <h2 title="data.description">{{ data.temp }} <span>°C</span></h2>
    </div>
</template>

<script>
export default {
  name: 'WeatherData',
  props: {
      data: {
          type: Object
      },
      city: {
          type: String
      }
  },
  watch: {
      data() {
          this.data.temp = parseFloat(this.data.temp).toFixed(1).replace('.', ',');
      }
  }
}
</script>

<style scoped>
    .weatherdata {
        padding: 2em 2.5em;
    }
    h1 {
        margin: 0;
        text-transform: uppercase;
        color: #FFE46B;
        position: relative;
        line-height: 1;
    }
    h1:after {
        content: "";
        position: absolute;
        width: 100%;
        background-color: rgb(94, 101, 255);
        height: 15px;
        bottom: 0;
        left: 0;
        z-index: -1;
    }
    h2 {
        margin-bottom: 0;
        margin-top: 10px;
        color: #fff;
    }
    h2 span {
        color: #FFE46B;
    }
</style>