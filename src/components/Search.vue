<template>
    <div class="searchform">
        <form v-on:submit.prevent="onSubmit" v-if="showForm">
            <button v-on:click.prevent="toggleForm">Hide</button>
            <input type="text" name="search" id="search" ref="search" placeholder="Search for your city" autofocus />
            <input type="submit" value="Search" />
        </form>
        <div class="searchform--toggle" v-if="!showForm">
            <button v-on:click="toggleForm">Need a different location?</button>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Search',
  data() {
      return {
          city: '',
          showForm: false
      }
  },
  methods: {
      onSubmit() {
          this.$emit('getCity', this.$refs.search.value);
      },
      toggleForm() {
          this.showForm = !this.showForm;
      }
  }
}
</script>


<style scoped>
.searchform,
.searchform--toggle {
    position: fixed;
    left: 0;
    right: 0;
    margin: auto;
    bottom: 0;
    width: 80%;
    text-align: center;
    display: flex;
    flex-wrap: wrap;
}
.searchform button,
.searchform--toggle button {
    display: block;
    width: 100%;
    text-align: center;
    margin-bottom: 0.5em;
}
.searchform--toggle {
    max-width: 600px;
    text-align: center;
}
button {
    background: none;
    border: none;
    color: #fff;
    font-weight: bold;
    font-size: 1.1em;
    cursor: pointer;
}
button:hover {
    color: #FFE46B;
}
form {
    width: 100%;
    max-width: 600px;
    margin: auto;
}
input[type="text"] {
    min-width: 400px;
}
input[type="submit"] {
    color: #fff;
    background-color: blueviolet;
    cursor: pointer;
    font-weight: bold;
}
input[type="submit"]:hover {
    background-color: blue;
}
input {
    text-transform: uppercase;
    font-size: 1.2em;
    padding: 0.5em 1.5em;
    border: none;
    box-shadow: none;
}
</style>
