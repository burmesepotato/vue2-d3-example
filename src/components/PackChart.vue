<template>
  <div>
    <h1>Circle Pack in D3</h1>
    <h2>{{ message }}</h2>

    <svg :width="width" :height="height">
      <g transform="translate(50,50)">
        <circle
          v-for="c in output"
          :key="c.id"
          :r="c.r"
          :cx="c.x"
          :cy="c.y"
          :fill="c.fill"
          :stroke="c.stroke"
        />
      </g>
    </svg>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: "PackChart",
  props: {
    tweetData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      message: "👋 from the Chart Component",
      height: 600,
      width: 600,
    };
  },
  created() {
    this.colourScale = d3
      .scaleOrdinal()
      .range(["#fcaa67", "#b0413e", "#ffffc7", "#548687"]);
  },
  computed: {
    packData() {
      // restructure the data to use it as a hierarchy
      const nestedTweets = d3
        .nest()
        .key((d) => d.user)
        .entries(this.tweetData);

      const packableTweets = { id: "All Tweets", values: nestedTweets };

      return d3
        .hierarchy(packableTweets, (d) => d.values)
        .sum((d) =>
          d.retweets ? d.retweets.length + d.favorites.length + 1 : 1
        );
    },
    output() {
      return this.packChart();
    },
  },
  methods: {
    packChart() {
      /* 
        to draw something on the screen, 
        we need to return some data 
        that can be bound to the SVG in the template
      */
      const packChart = d3.pack();
      packChart.size([500, 500]);
      packChart.padding(10);
      const output = packChart(this.packData).descendants();
      return output.map((d, i) => {
        const fill = this.colourScale(d.depth);
        return {
          id: i + 1,
          r: d.r,
          x: d.x,
          y: d.y,
          fill,
          stroke: "grey",
        };
      });
    },
  },
};
</script>

<style lang="scss" scoped>
</style>