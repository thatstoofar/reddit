<template>
<html>
<head>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">

</head>
<body>
  <div id="app">
    <nav class="navbar navbar-dark bg-primary">
  <!-- Navbar content -->
      r/popular
    </nav>
    
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


   <div v-infinite-scroll="loadOlderStories" infinite-scroll-disabled="busy" infinite-scroll-distance="10"></div>
  </div>
</body></html>
</template>

<script>
export default{
  name: 'app',
  methods: {
        loadMore: function() {
          console.log('inf');

          this.busy = true;
          setTimeout(() => { for (var i = 0, j = 10; i < j; i++) {
            this.data.push({ name: count++ });
          }
          this.busy = false;
        }, 1000);

        },

        loadStories: function() {
          var stories = [];
          this.$http.get('https://www.reddit.com/r/popular/new.json').then(response => {
            response.body.data.children.map(function(value,key){
              if(!value.data.thumbnail ||value.data.thumbnail === 'self' || value.data.thumbnail ==='default'){
                value.data.thumbnail = 'http://www.freepngimg.com/thumb/mario/20698-7-mario-transparent-background-thumb.png'
                console.log('replace')
              }
              stories.push(value.data)
            })
          })
          this.stories = stories; 
        },

        loadOlderStories: function() {
          var stories = [];
          var params = {};

          params['after'] = 't3_8gr1xe'

          console.log([params]);
          /*
          if(this.stories.length > 0) {
            params['after'] = this.stories[this.stories.length-1].name;
          } */
          this.$http.get('https://www.reddit.com/r/popular/new.json', params).then(response =>{
            response.body.data.children.map(function(value,key){
              if(!value.data.thumbnail ||value.data.thumbnail === 'self' || value.data.thumbnail ==='default'){
                value.data.thumbnail = 'http://www.freepngimg.com/thumb/mario/20698-7-mario-transparent-background-thumb.png'
              }
              stories.push(value.data)
            })
          })
          this.stories = stories
      }
    },
  data(){
    return {
      msg: 'Wel',
      stories: [],
      list: [],
    }

  },
  mounted(){
    this.loadOlderStories();
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
  display: block;
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
</style>
