<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Media-Library</a>
    <button
      class="navbar-toggler"
      type="button"
      data-toggle="collapse"
      data-target="#navbarSupportedContent"
      aria-controls="navbarSupportedContent"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item active">
          <router-link class="nav-link" to="/">Home</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/about">Library</router-link>
        </li>
      </ul>
      <form class="form-inline my-2 my-lg-0" @submit.prevent="search">
        <input
          id="searchterm"
          class="form-control mr-sm-2"
          type="search"
          placeholder="Search"
          aria-label="Search"
        />
        <button class="btn btn-outline-success my-2 my-sm-0" @click="search">
          Search
        </button>
      </form>
    </div>
  </nav>

  <div class="container">
    <div class="row" id="cardcontainer">
      <div class="card col-md-4 my-3" style="width: 18rem" v-for="data in rst">
        <img class="card-img-top" :src="data.urls.thumb" alt="Card image cap" />
        <div class="card-body">
          <h5 class="card-title">{{ data.alt_description }}</h5>
          <p class="card-text">
            {{ data.description }}
          </p>

            <button type="submit"
            class="btn btn-primary float-right my-2 mx-2"
            @click="
              addData(data.urls.thumb, data.alt_description, data.description)
            "
          >
            Add
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

var mode = 0;
var photos = [];
var title = "";

export default {

  name: "App",
  data() {
    return {
      photo: [],
      json: [],
      accessKey: "MQTSzBkXjG9biUave6AEqFAWO1RKhxjKBH-_s_oDEZQ",
      url: "https://api.unsplash.com/search/photos",
      res: "",
      searchterm: "tree",
      rst: [],
    };
  },
  //https://api.unsplash.com/search/photos?query=tree&client_id=MQTSzBkXjG9biUave6AEqFAWO1RKhxjKBH-_s_oDEZQ&per_page=2
  methods: {
    addData(url, titl, desc) {
      var data =
        '{ "url":"' +
        url +
        '" , "title":"' +
        titl +
        '", "description":"' +
        desc +
        '", "id":"'+this.json.length +'" }';
      console.log(this.json.length)
      var j = window.sessionStorage.getItem('p')
      var s = "[";
      s+= j + "]";
      
      this.json.push(data);
      console.log(this.json)
      window.sessionStorage.setItem('p',this.json)
    },
    search: function () {
      fetch(
        "https://api.unsplash.com/search/photos?query=" +
          document.getElementById("searchterm").value +
          "&client_id=MQTSzBkXjG9biUave6AEqFAWO1RKhxjKBH-_s_oDEZQ&per_page=40"
      )
        .then((response) => {
          return response.json();
        })
        .then((parsedJson) => {
          console.log(parsedJson.results);
          this.rst = parsedJson.results;
        });
    },
  },
};


//https://api.unsplash.com/search/photos?query=tree&client_id=MQTSzBkXjG9biUave6AEqFAWO1RKhxjKBH-_s_oDEZQ&per_page=2
</script>
