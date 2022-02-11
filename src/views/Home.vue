<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      places: [],
      params: {},
      currentPlace: {},
      params2: {},
    };
  },
  created: function () {
    this.indexPlace();
  },
  methods: {
    indexPlace: function () {
      console.log("index test");
      axios.get("http://localhost:3000/places").then((response) => {
        console.log(response.data);
        this.places = response.data;
      });
    },
    createPlace: function () {
      axios
        .post("http://localhost:3000/places", this.params)
        .then((response) => {
          console.log(response.data);
          this.places.push(response.data);
        });
      this.indexPlace();
    },
    showPlace: function (place) {
      console.log(place);
      this.currentPlace = place;
      this.params2 = place;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      axios
        .patch(`http://localhost:3000/places/${place.id}`, place)
        .then((response) => {
          console.log(response.data);
        });
    },
    destroyPlace: function (place) {
      if (confirm("Are you sure you want to delete this place?")) {
        console.log("Delete Test");
        axios
          .delete(`http://localhost:3000/places/${place.id}`)
          .then((response) => {
            console.log(response.data);
            var index = this.places.indexOf(place);
            this.places.splice(index, 1);
          });
      }
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
  </div>
  <div>
    <h1>Create New Place</h1>
    <p>{{ params }}</p>
    <div>Name: <input type="text" v-model="params.name" /></div>
    <div>Address: <input type="text" v-model="params.address" /></div>
  </div>
  <button v-on:click="createPlace()">Create</button>
  <div v-for="place in places" v-bind:key="place.id">
    <h2>{{ place.name }}</h2>
    <p>{{ place.address }}</p>
    <button v-on:click="showPlace(place)">Show Info</button>
    <div>
      <dialog id="place-details">
        <form method="dialog">
          <h2>Place Info</h2>
          <p>Name: <input type="text" v-model="params2.name" /></p>
          <p>Address: <input type="text" v-model="params2.address" /></p>
          <p>{{ place.address }}</p>
          <button>Close</button>
          <button v-on:click="updatePlace(currentPlace)">Update</button>
          <button v-on:click="destroyPlace(currentPlace)">Delete</button>
        </form>
      </dialog>
    </div>
  </div>
</template>

<style></style>
