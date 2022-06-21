<template>
  <div>
    <PackChart v-bind:tweetData="tweets"/>
  </div>
</template>

<script>
import * as d3 from 'd3'
import PackChart from './components/PackChart.vue'
export default {
  name: 'App',
  components: {
    PackChart
  },
  data(){
    return{
      tweets: [],
      fname:"Arjun"
    }
  },
  mounted(){
    this.fetchTweets()
  },
  methods:{
     async fetchTweets() {
      let data =  await d3.json("./tweets.json")
      this.tweets = data
      console.log(this.tweets)
    }
  },
  components:{
    PackChart
}
}
</script>

<style>

</style>
