<template>
    <div>
        <h1>Circle PACK Chart</h1>
        <h2>{{subtitle}}</h2>
        <svg :height="height" :width="width">
            <g transform="translate(50,50)">
                <circle 
                    v-for="c in output" 
                    :key = "c.id"
                    :r = "c.r"
                    :cx ="c.x"
                    :cy = "c.y"
                    :fill = "c.fill"
                    :stroke = "c.stroke">
                  
                    </circle>
          
            </g>
        </svg>
       
    </div>
</template>

<script>
import * as d3 from 'd3'
export default {
    name:"PackChart",
    props: ["tweetData"],
    data(){
        return{
            subtitle: "Its from d3 Charts",
            height: 800,
            width: 800
        }
    },
    created(){
        this.colourScale = d3.scaleOrdinal().range(['#5EAFC6', '#FE9922','#93c464', '#75739F'])
    },
    methods: {
        packChart(){
            const packChart = d3.pack()
            packChart.size([500,500])
            packChart.padding(10)
            const output = packChart(this.packData).descendants()

            return output.map((d, i)=> {
                const fill = this.colourScale(d.depth)
                return {
                    id: i+1,
                    r: d.r,
                    x: d.x,
                    y: d.y,
                    fill,
                    stroke: "green"
                }
            }) 
        }
    },
    computed:{
        packData(){
            
            console.log(this.tweetData)
            const nestedTweets = d3.nest().key(d => d.user).entries(this.tweetData)
            console.log(nestedTweets)
            const packableTweets = {id: "All Tweets", values:nestedTweets}
            console.log(packableTweets.values)
            return d3.hierarchy(packableTweets, d => d.values)
                     .sum(d => d.retweets ? d.retweets.length + d.favorites.length+ 1: 1)

        },
        output(){
            return this.packChart()
            
        }
    }
}
</script>

<style>
</style>
