<template>
  <div class="root">
    <div class="scroll-up-indicator">
      <strong>
        scroll<br />
        down<br />
        &#8675;
      </strong>
    </div>
    <div class="scroll-down-indicator">
      <strong>
        or<br />
        scroll<br />
        back<br />
        &#8673;
      </strong>
    </div>
    <div class="d3-root" ref="d3Root"></div>
    <IntersectionRoot
      :threshold="threshold"
      :rootMargin="rootMargin"
      class="intersection-root"
    >
      <IntersectionChild @enter="hideCircle" class="intersection-child">
        <p>
          <i>Start scrolling...</i>
        </p>
      </IntersectionChild>
      <IntersectionChild @enter="drawCircle" class="intersection-child">
        <p>
          <i>I will draw a red circle &#8595;</i>
        </p>
      </IntersectionChild>
      <IntersectionChild @enter="fillBlue" class="intersection-child">
        <p>
          <i>I will fill it with blue &#8595;</i>
        </p>
      </IntersectionChild>
      <IntersectionChild @enter="shrink" class="intersection-child">
        <p>
          <i>I will make it disappear!</i>
        </p>
      </IntersectionChild>
    </IntersectionRoot>
  </div>
</template>

<script>
import { IntersectionRoot, IntersectionChild } from "vue-intersection";
import * as d3 from "d3";

const dimension = {
  width: 800,
  height: 500,
};
let circle = null;
const transition = d3
  .transition()
  .duration(1000)
  .ease(d3.easeLinear);

export default {
  name: "ObservedContainer",
  components: { IntersectionRoot, IntersectionChild },
  data() {
    return {
      threshold: 0.5,
      rootMargin: "0px",
      // debounce: 6000,
    };
  },
  methods: {
    hideCircle() {
      circle.transition(transition).style("stroke-width", "0");
    },
    drawCircle() {
      circle
        .transition(transition)
        .style("stroke", "red")
        .style("stroke-width", "10px")
        .style("fill", "white");
    },
    fillBlue() {
      circle
        .transition(transition)
        .style("fill", "blue")
        .attr("r", 50);
    },
    shrink() {
      circle.transition(transition).attr("r", "0");
    },
  },
  mounted() {
    const svg = d3
      .select(this.$refs.d3Root)
      .append("svg")
      .attr("viewBox", "0 0 800 500");
    circle = svg
      .append("circle")
      .attr("cx", dimension.width / 2)
      .attr("cy", dimension.height / 2)
      .attr("r", 50)
      .style("fill", "white")
      .style("opacity", "0.7");
  },
};
</script>

<style scoped>
.root {
  position: relative;
  max-width: 800px;
  margin: 100px auto;
  border: 1px solid black;
  height: 500px;
  background-color: white;
}
.intersection-root {
  width: 100%;
  height: 100%;
  overflow: scroll;
  position: absolute;
  top: 0;
  left: 0;
  /* Firefox */
  scrollbar-width: none;
}
.intersection-root::-webkit-scrollbar {
  /* Safari and Chrome */
  display: none;
}
.intersection-child {
  width: 100%;
  height: 100%;
  justify-content: center;
  align-items: center;
}
.intersection-child:nth-child(1) {
  display: flex;
}
.scroll-up-indicator {
  position: absolute;
  color: cornflowerblue;
  right: 15px;
  top: 10px;
  animation-duration: 0.5s;
  animation-iteration-count: infinite;
  animation-direction: alternate;
  animation-name: breathing;
}
.scroll-down-indicator {
  position: absolute;
  color: cornflowerblue;
  left: 15px;
  bottom: 10px;
  animation-duration: 0.5s;
  animation-iteration-count: infinite;
  animation-direction: alternate;
  animation-name: breathing;
}
@keyframes breathing {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
.d3-root {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
}
</style>
