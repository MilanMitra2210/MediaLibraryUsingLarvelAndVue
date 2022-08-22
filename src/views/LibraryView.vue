<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Media-Library</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent"
      aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
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
    </div>
  </nav>

  <div class="container">
    <div class="row" id="cardcontainer">
      <div v-for="data in posts">

      <div class="col-md-4 my-3">
        <div class="card" style="width: 18rem;">
          <img class="card-img-top" :src="data.url" alt="Card image cap">
          <div class="card-body">
              <h5 class="card-title" :id="'title'+data.id">{{data.title}}</h5>
              <input class="form-control form-control-sm my-2" :id="'input'+data.id" hidden  type="text" placeholder="Edit Title">
              <p  class="card-text" :id="'desc'+data.id">{{data.description}}</p>
              <textarea class="form-control form-control-sm my-2" name="" :id="'textarea'+data.id" hidden cols="30" rows="10"  placeholder="Edit Description"></textarea>
              <button type="button" :id="'sb'+data.id" class="btn btn-primary float-right mx-2 my-2"
                @click="editContent(data.id)">Edit</button>
              <button type="submit" :id="'sv'+data.id" class="btn btn-primary float-right mx -2 my-2" @click="updateData(data.id)" hidden>Save</button>
          </div>
        </div>
      </div>
        
        </div>
    
    </div>
  </div>
</template>

<script>
export default {
  name: "LibraryView",
  data() {
    return {
      posts:'',
      idx:0
    }
  },
  mounted:function(){
    this.posts = window.sessionStorage.getItem('p')
    var s = "[" + this.posts + "]";
    this.posts = JSON.parse(s);
    console.log(this.posts)
  },  
  methods: {
    updateData(id) {
      document.getElementById("sb" + id).hidden=false;
      document.getElementById("sv" + id).hidden=true;
      document.getElementById("title" + id).hidden=false;
      document.getElementById("desc" + id).hidden=false;
      document.getElementById("input" + id).hidden=true;
      document.getElementById("textarea" + id).hidden=true;

     var newtitle = document.getElementById('input'+id).value;
     var newdescp = document.getElementById('textarea'+id).value;

    var temp = window.sessionStorage.getItem('p');
    var s = "[" + temp +"]"
    temp = JSON.parse(s)
    temp[id].title = newtitle;
    temp[id].description = newdescp;
    

    temp = JSON.stringify(temp);
    temp = temp.slice(1, -1);
    console.log(temp)
    window.sessionStorage.setItem('p',temp)
    location.reload();

    },
    editContent(id) {
      console.log(id)
      document.getElementById("sb" + id).hidden=true;
      document.getElementById("sv" + id).hidden=false;
      document.getElementById("title" + id).hidden=true;
      document.getElementById("desc" + id).hidden=true;
      document.getElementById("input" + id).hidden=false;
      document.getElementById("textarea" + id).hidden=false;
    }
  }
}



</script>