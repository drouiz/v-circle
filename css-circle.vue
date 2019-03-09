<template>
    <div class="donut-size">
       <div v-bind:class="[donut ? 'donut-graph' : 'pie-graph', 'pie-wrapper']">
        <span class="label">
            <span v-bind:class="icon" v-bind:style="{ color :  color1 }"></span>
        </span>
        <div class="pie" v-bind:style="{clip: pie}">
          <div class="left-side half-circle" v-bind:style="{transform: leftside, border :  '0.1em solid ' + color1 } "></div>
          <div class="right-side half-circle" v-bind:style="{transform: rightside, border :  '0.1em solid ' + color1}"></div>
        </div>
        <div class="shadow"></div>
      </div>
    </div>
</template>

<script>
export default {
      name:'cssCircle',
      props: {
        percent: Number,
        icon: String,
        color1: String
      },
      data() {
        return {
          donut: true
        }
      },
      watch: {
          'percent': function () {
              // Restricte range of acceptable percents to whole numbers between 0-100
              var wholeNumberValue = Math.round(this.percent);
              var restrictedUpperValue = Math.min(wholeNumberValue, 100);
              var restrictedLowerValue = Math.max(restrictedUpperValue, 0);
              this.percent = restrictedLowerValue;
          }
      },
      computed: {
          'pie': function () {
              var pie = 'rect(0, 1em, 1em, 0.5em)';
              if (Math.round(this.percent) > 50) {
                  pie = 'rect(auto, auto, auto, auto)';
              }
              return pie;
          },
          'leftside': function () {
              var degrees = Math.round(360 * (this.percent / 100));
              var rotation = 'rotate(' + degrees + 'deg)';
              return rotation;
          },
          'rightside': function () {
              var rotation = 'rotate(0deg)';
              if (Math.round(this.percent) > 50) {
                  rotation = 'rotate(180deg)';
              }
              return rotation;
          }
      }
}
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Lato:700");

.donut-size {
  font-size: 4em;
  display: inline-block;
}

.pie-wrapper {
  position: relative;
  width: 1em;
  height: 1em;
  margin: 0px auto;
  font-family: "Lato", Tahoma, Geneva, sans-serif;
}
.pie-wrapper .pie {
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
  clip: rect(0, 1em, 1em, 0.5em);
}
.pie-wrapper .half-circle {
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
  border: 0.1em solid #1abc9c; /* cambiar este color */
  border-radius: 50%;
  clip: rect(0em, 0.5em, 1em, 0em);
  box-sizing: border-box;
}
.pie-wrapper .right-side {
  transform: rotate(0deg);
}
.pie-wrapper .label {
  position: absolute;
  top: 0.35em;
  right: 0.02em;
  bottom: 0em;
  left: 0.19em;
  display: block;
  background: none;
  border-radius: 50%;
  color: #7F8C8D;  /* cambiar este color */
  font-size: 0.50em;
  line-height: 1.3em;
  text-align: center;
  cursor: default;
  z-index: 2;
}
.pie-wrapper .smaller {
  padding-bottom: 20px;
  color: #BDC3C7;
  font-size: 0.45em;
  vertical-align: super;
}
.pie-wrapper .shadow {
  width: 100%;
  height: 100%;
  border: 0.1em solid #BDC3C7;
  border-radius: 50%;
  box-sizing: border-box;
}

.pie-wrapper.donut-graph .right-side,
.pie-wrapper.donut-graph .left-side,
.pie-wrapper.donut-graph .shadow {
  border-width: 0.1em;
}

.pie-wrapper.pie-graph .right-side,
.pie-wrapper.pie-graph .left-side,
.pie-wrapper.pie-graph .shadow {
  border-width: 0.5em;
}
.pie-wrapper.pie-graph .smaller {
  color: #7F8C8D;
}

#controls {
  padding-top: 10px;
  font-family: sans-serif;
  font-size: 2em;
  text-align: center;
}
#controls input {
  display: block;
  margin: 20px auto 0px;
}
#controls input[type="number"] {
  font-size: 1em;
  width: 125px;
  text-align: center;
}
#controls input[type="checkbox"] {
  display: inline;
  width: 1.4em;
  height: 1.4em;
}
</style>
