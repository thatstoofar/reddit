<template>
<html>
<head>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
</head>
<body>
  <div id="app">
    <h1 class= 'title'> /r/popular </h1>
    <div class = 'item'>

      <a class= 'link'
      a v-bind:href=story.url
       v-for='story in stories'
       target='_blank'
      >
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
      </div> 
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

        loadStories: function(){
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
    this.loadStories();
  }, 
}
</script>


<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h2 {
  align-content: left;
  font-weight: normal;
}
img{
  max-width: 100%;
  max-height: 100%;
  border:none;
  outline:none;

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
  border-style: solid;
  border-width: 10px;
}
.thumbnail{
  align-content: center;
}
.title{
  font-size: 50px;
  font-color: red;
  align-content: center;
}
.story-title{
  align-content: right;
}
.time-ago{
  align-content: left;
}
</style>
