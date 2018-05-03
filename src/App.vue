<template>
<html>
<head>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">

</head>
<body>
  <div id="app">
    <div class='header rounded-0 bg-secondary border border-secondary '>
    <ul>
      <li></li>
      <li></li>
    </ul>
    </div>
    <nav class="navbar navbar-dark bg-primary rounded-0">
  <!-- Navbar content -->
      r/movies <!-- {{stories[0].subreddit}} -->
    </nav>
    
    <div class= 'top-bar'>
      <div class ='next-page'> 
        <p>view more:</p>
        <button type="button" class="btn" v-on:click="loadPrevPage">Back</button>
        <button type="button" class="btn" v-on:click="loadOlderStories">Next</button>
      </div>
      <div class ='next-page'> 
        <p>Limit Number of items:</p>
        <button type="button" class="btn" v-on:click="setLimit(5)">5</button>
        <button type="button" class="btn" v-on:click="setLimit(10)">10</button>
        <button type="button" class="btn" v-on:click="setLimit(25)">25</button>
        <button type="button" class="btn" v-on:click="setLimit(50)">50</button>
      </div>
    </div>
    <div class = 'item'>
      <a class= 'link' a v-bind:href=story.url v-for='story in stories' target='_blank'>
      <div class = 'flex'>
        <div class='thumbnail'> 
          <img v-bind:src=story.thumbnail>
        </div>
        <div class='story-title'>
          <h2>{{story.title}}</h2>
          <div class='time-ago'>
          <p> <span>{{ story.created_utc | moment("from", "now") }}</span>- {{story.domain}} </p>
          </div> 
        </div> 
        </ul>
      </div> 
      <hr>
      </a>
    </div>
    <div class= 'top-bar'>
      <div class ='next-page'> 
        <p>view more:</p>
        <button type="button" class="btn" v-on:click="loadPrevPage">Back</button>
        <button type="button" class="btn" v-on:click="loadOlderStories">Next</button>
      </div>
      <div class ='next-page'> 
        <p>Limit Number of items:</p>
        <button type="button" class="btn" v-on:click="setLimit(5)">5</button>
        <button type="button" class="btn" v-on:click="setLimit(10)">10</button>
        <button type="button" class="btn" v-on:click="setLimit(25)">25</button>
        <button type="button" class="btn" v-on:click="setLimit(50)">50</button>
      </div>
    </div>
    <div class='footer rounded-0 bg-secondary border border-secondary '>
    <ul>
      <li></li>
      <li></li>
    </ul>
    </div>

   <!--<div v-infinite-scroll="loadOlderStories" infinite-scroll-disabled="busy" infinite-scroll-distance="10"></div> -->
  </div>
</body>
</html>
</template>

<script>
export default{
  name: 'app',
  methods: {
        loadMore: function() {
          this.busy = true;
          setTimeout(() => { for (var i = 0, j = 10; i < j; i++) {
            this.data.push({ name: count++ });
          }
          this.busy = false;
        }, 1000);

        },

        loadStories: function() {
          var stories = [];
          var params = {params:{}}
          params.params.limit =  this.limit[0] 
          this.$http.get(this.url, params).then(response => {
            response.body.data.children.map(function(value,key){
              if(!value.data.thumbnail ||value.data.thumbnail === 'self' || value.data.thumbnail ==='default'){
                value.data.thumbnail = 'http://www.freepngimg.com/thumb/mario/20698-7-mario-transparent-background-thumb.png'
              }
              stories.push(value.data)
            })
          })
          this.stories = stories; 
        },

        loadOlderStories: function() {
          var stories = [];
          var params = {params:{}};



          console.log(this.url);
          
          if(this.stories.length > 0) {
            params.params.after = this.stories[this.stories.length-1].name;
          } 
          params.params.limit =  this.limit[0] 
          this.$http.get(this.url, params).then(response =>{
            response.body.data.children.map(function(value,key){
              if(!value.data.thumbnail ||value.data.thumbnail === 'self' || value.data.thumbnail ==='default'){
                value.data.thumbnail = 'http://www.freepngimg.com/thumb/mario/20698-7-mario-transparent-background-thumb.png'
              }
              stories.push(value.data)
            })
          })
          this.stories = stories
      },

      loadPrevPage: function(){
        var stories = [];
        var params = {params:{}};

          
          if(this.stories.length > 0) {
            params.params.after = this.stories[0].name;
          } 
          params.params.limit =  this.limit[0] 

          this.$http.get(this.url, params).then(response =>{
            response.body.data.children.map(function(value,key){
              if(!value.data.thumbnail ||value.data.thumbnail === 'self' || value.data.thumbnail ==='default'){
                value.data.thumbnail = 'http://www.freepngimg.com/thumb/mario/20698-7-mario-transparent-background-thumb.png'
              }
              stories.push(value.data)
            })
          })
          this.stories = stories
      },
      setLimit: function(limit){
        this.limit[0] = limit
        this.loadStories()
      }

    },
  data(){
    return {
      url: 'https://www.reddit.com/r/movies/new.json',
      stories: [],
      list: [],
      limit: [],
    }

  },
  mounted(){
    this.loadStories()
  }, 
}
</script>


<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
body{
  font-size: 16px;
}

h2 {
  align-content: left;
  font-weight: normal;
}
img{
  height: 100px;
  width: 100px;

}

ul {
  list-style-type: none;
  padding: 0;
}

li {  
  display: inline-block;
  margin: 50px 0;
  white-space: normal;
}

a {
  color: #42b983;
  text-decoration: none;
}
p{
  align-content: left;
}

.flex{
  display: flex;
  padding-bottom: auto;
  align-content: left;
  padding-left: 10px;
}
.thumbnail{
  align-content: center;
  height: 100px;
  width: 100px;
  border: 0px;
}
.title{
  font-size: 50px;
  font-color: red;
  align-content: center;
}
.story-title{
  align-content: right;
  padding-left: 20px;
}
.time-ago{
  align-content: left;
  padding-top: 0.5%px;
}
.navbar {
    font-size: 35px;
    margin-bottom: 0;
    height: auto;
    width: auto;
}
.next-page{
  padding-left:1%;
  padding-bottom:1%;
}
.btn{
  padding-left:100px;
}
.top-bar{
  display:flex;
}

</style>
