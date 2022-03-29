<template lang="pug">
foreignObject(
  :x="rectangleArea.x"
  :y="rectangleArea.y"
  :width="rectangleArea.width"
  :height="isClicked ? rectangleArea.height : (rectangleArea.height-marginDiv)/4"
)
  div(
    :style="{width: `${rectangleArea.width}px`, maxHeight:`${(rectangleArea.height-marginDiv)/4}px`}"
    class="circleTitle btn btn-secondary card-title"
    @click="show"
  )
    p(
      class="circleTitleText"
    ) {{ circle.data.name }}
  div(
    v-if="isClicked && (circle.data.description || circle.data.business_name)"
    :style="{width: `${rectangleArea.width}px`, maxHeight: `${(rectangleArea.height-marginDiv)/4 * 3}px`}"
    class="circleDescription card-body"
    @click="isClicked=false"
  )
    p(class="card-text") {{ circle.data.description || circle.data.business_name }}
</template>

<script>
export default {
  name: "CircleInfo",
  props: {
    circle: Object
  },
  data() {
    return {
      isClicked: false,
      marginDiv: 10,
    }
  },
  computed: {
    rectangleArea() {
      return this.getRect(this.circle)
    }
  },
  methods: {
    show() {
      this.isClicked = !this.isClicked
    },
    getRect(c) {
      const angle = 90
      const angX = (180 - angle) / 2 * 3 + angle
      const angY = angX + angle
      const x1 = c.r * Math.cos(angX * (Math.PI / 180)) + c.x
      const y1 = c.r * Math.sin(angX * (Math.PI / 180)) + c.y

      const x2 = c.r * Math.cos(angY * (Math.PI / 180)) + c.x
      const y2 = c.r * Math.sin((angX - 180 + angle) * (Math.PI / 180)) + c.y
      const width = Math.abs(x1 - x2)
      const height = Math.abs(y1 - y2)
      return {
        x: x1 + 2,
        y: y1 + 5,
        height: height - 10,
        width: width - 4
      }
    }
  }
}
</script>

<style scoped lang="scss">
.circleTitleText {
  text-align: center;
  margin: 0;
  padding: 0;
}

@media screen and (max-width: 600px) {
  .circleTitle {
    font-size: 7px;
    font-weight: normal;
  }
  .circleDescription {
    font-size: 6px;
  }
}

@media screen and (min-width: 601px) {
  .circleTitle {
    font-size: 15px;
  }
  .circleDescription {
    font-size: 12px;
  }
}


.circleTitle {
  font-weight: 600;
  text-align:center;
  white-space: pre-wrap;
  display: block;
  overflow-y: auto;
  overflow-x: hidden;
  word-break:break-all;
  -webkit-overflow-scrolling: touch;
  padding: 0;
}

.circleDescription {
  padding: 5px;
  background: white;
  text-align:center;
  color: black;
  overflow-y: auto;
  overflow-x: hidden;
  margin-top: 10px;
  -webkit-overflow-scrolling: touch;
}

.circleDescription, .circleTitle {
  overflow: auto;
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.circleDescription::-webkit-scrollbar, .circleTitle::-webkit-scrollbar {
  width: 0;
  height: 0;
}

foreignObject {
  transition: 0.4s linear;
}
</style>