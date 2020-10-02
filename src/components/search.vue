<template>
  <div class="hello">
    <h1>FURKAN KARAKUZU</h1>
    <div class="arama">
      <input type="text" v-model="query" @keyup.enter="search(1)"> <br>
      <button @click="search(1)">Furkan ile Ara</button>
      <button @click="searchImg" class="imgAra">Furkan ile Görsel Ara</button>
    </div>
    <br><br>
    <div v-if="querySearch">
      <div v-for="item in items" :key="item.title" class="sonuc">
        <a v-bind:href="item.link" target="_blank"> <b class="siteBaslik">{{item.title}}</b> </a> <br>
        <span>{{item.snippet}}</span> <br>
        <a v-bind:href="item.link" target="_blank"> {{item.link}}  </a> <br>
      </div>
    </div>
    <div v-if="queryImgSearch">
      <div v-for="item in itemsImg" :key="item.title" class="imgSonuc">
        <a :href="item.link"><img :src="item.pagemap.cse_image[0].src" :alt="item.title" target="_blank"></a>
      </div>
    </div>
      <ul class="pagination" v-if="querySearch">
        <li @click="search(1)">1</li>
        <li @click="search(2)">2</li>
        <li @click="search(3)">3</li>
        <li @click="search(4)">4</li>
        <li @click="search(5)">5</li>
        <li @click="search(6)">6</li>
        <li @click="search(7)">7</li>
        <li @click="search(8)">8</li>
        <li @click="search(9)">9</li>
        <li @click="search(10)">10</li>
      </ul>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'search',
  data(){
    return{
      items : null,
      itemsImg : null,
      query:'',
      querySearch : false,
      queryImgSearch : false,
    }
  },
  methods : {
    search(start){
      axios.get('https://customsearch.googleapis.com/customsearch/v1?cx=YOUR_CX_CODE&q='+this.query+'&key=YOUR_API_KEY&start='+start+'&num=10')
        .then( res=>{
          this.queryImgSearch=false
          this.querySearch=true
          this.items=null
          this.items=res.data.items
      })
      scroll(0, 0)
    },
    searchImg(){
      this.queryImgSearch=true
      this.querySearch=false
      this.itemsImg=[]
      for (let i = 1; i < 52; i+=10) {
        axios.get('https://customsearch.googleapis.com/customsearch/v1?cx=YOUR_CX_CODE&q='+this.query+'&key=YOUR_API_KEY&start='+i+'&num=10')
        .then( res=>{
          res.data.items.forEach(item => {
            if(item.pagemap.cse_image){
              this.itemsImg.push(item)
            }
          })
        })
      }
      scroll(0, 0)
    }
  }
}
</script>


<style scoped>
h1{
  text-align: center;
}
.arama{
  padding-top: auto;
  padding-bottom: auto;
  width: 100%;
  text-align: center;
}
.arama input{
  height: 30px;
  width: 45%;
  padding: 5px;
  font-size: 1em;
  border-radius: 10px;
}
.arama button{
  margin-top: 10px;
  padding:14px 30px;
  border: 0 solid gray;
  border-radius: 10px;
  font-weight: bold;
}
.arama button:hover{
  border: 1px solid gray;
  background-color: lightskyblue;
  color: white;
  cursor: pointer;
}
.imgAra{
  margin-left: 15px;
}
.sonuc a{
  text-decoration: none;
}
.sonuc a:hover{
  text-decoration: underline;
}
.sonuc{
  width: 75%;
  margin-top: 20px;
  margin-right: auto;
  margin-left: auto;
  margin-bottom: 30px;
  font-size: .9em;
}
.sonuc:hover{
  background-color: lightgray;
}
.imgSonuc{
  display:inline-block;
  padding-left: 20px;
  height: 240px;
}
.imgSonuc img{
  float:left;
  width: 200px;
  height: 200px;
  margin: 20px;
}
.imgSonuc:hover{
  background-color: lightgray;
}
.siteBaslik{
  font-size: 1.5em;
}
.pagination{
  margin-left:9.5%;
}
.pagination li{
  text-align: center;
  list-style-type: none;
  float: left;
  margin-right:2%;
  margin-bottom: 20px;
  color: blue;
  cursor: pointer;
  width: 40px;
  height: 20px;
  background-color: lightgrey;
}
.pagination li:hover{
  text-decoration: underline;
}
</style>
