<template lang="pug">
nav(aria-label="breadcrumb" ref="navbar_breadcrumb")
  ol(class="breadcrumb")
    li(class="breadcrumb-item" :class="[path.length > 0 ? '' : 'active']" @click="clickMain") Root
    li(
      v-for="pathNode in path"
      :key="pathNode.id"
      class="breadcrumb-item"
      :class="[pathNode.id === path[path.length-1].id ? 'active' : '']"
      @click="clickToNode(pathNode)"
    ) {{ pathNode.text }}

svg(preserveAspectRatio="none" :viewBox="`0 0 ${width} ${height}`")
  g
    circle(
      v-if="rootNode.data"
      :cx="rootNode.x"
      :cy="rootNode.y"
      :r="rootNode.r"
      :stroke="rootNode.data.stroke ? rootNode.data.stroke : ''"
      :fill="rootNode.data.color"
      @click="clickBehind"
      class="mainCircle"
    )

    Circle(
      v-for="(c, key) in domains.children"
      :key="key"
      :c="c"
      @getChildren="click(c)"
    )
  CircleInfo(
    v-for="(c, key) in domains.children"
    :key="key"
    :circle="c"
  )
</template>

<script>
import {hierarchy, pack} from 'd3-hierarchy';
import Circle from "@/components/Circle";
import CircleInfo from "@/components/CircleInfo";


let d3 = {
  hierarchy: hierarchy,
  pack: pack
}

export default {
  name: "CirclePacking",
  components: {Circle, CircleInfo},
  props: {
    margin: {
      type: Object,
      default: function () {
        return {
          top: 20,
          right: 20,
          bottom: 20,
          left: 20
        }
      }
    }
  },
  data() {
    return {
      rootNode: {},
      path: [],
      navHeight: 0,
    }
  },
  computed: {
    domains() {
      return this.pack(this.$store.getters["business_domains/domains"])
    },
    width() {
      return document.documentElement.clientWidth - this.margin.right - this.margin.left;
    },
    height() {
      return document.documentElement.clientHeight - this.margin.top - this.margin.bottom - this.navHeight;
    },
  },
  mounted() {
    this.$store.dispatch("business_domains/getDomains").then((data) => {
      this.rootNode = this.pack(data)
    });
    this.navHeight =  this.$refs.navbar_breadcrumb.clientHeight;

  },
  methods: {
    clickMain() {
      this.$store.dispatch("business_domains/getDomains").then((data) => {
        this.rootNode = this.pack(data)
        this.path = []
      });
    },
    click(nod) {
      if (nod.data.has_children) {
        this.$store.dispatch("business_domains/getDomains", {id: nod.data.id}).then((data) => {
          this.rootNode = this.pack(data)
          this.path.push({text: this.rootNode.data.name, id: this.rootNode.data.id})
        });
      }
    },
    clickToNode(nod){
      let nodeIndexPath = this.path.indexOf(nod);
      this.$store.dispatch("business_domains/getDomains", {id: nod.id}).then((data) => {
          this.rootNode = this.pack(data)
          this.path = this.path.slice(0, nodeIndexPath+1)
        });
    },
    clickBehind() {
      this.path.pop()
      if (this.path.length > 0) {
        this.$store.dispatch("business_domains/getDomains", {id: this.path[this.path.length-1].id}).then((data) => {
          this.rootNode = this.pack(data)
        });
      } else {
        this.$store.dispatch("business_domains/getDomains").then((data) => {
          this.rootNode = this.pack(data)
        });
      }
    },
    pack(domainData) {
      let childrenLength = domainData.children ? domainData.children.length : 1
      let paddingCircles = childrenLength > 2 ? 5 : 20;
      let hierarchyData = d3.hierarchy(domainData)
          .sum((d) => d.value)
          .sort((a, b) => b.value - a.value)
      return d3.pack()
          .size([this.width, this.height])
          .padding(paddingCircles)(hierarchyData)
    }
  }
}
</script>

<style scoped>
circle {
  transition: 0.4s linear;
  fill-opacity: .7;
}
.breadcrumb-item {
  font-weight: 500;
}
.breadcrumb-item:hover {
  font-weight: 700;
}
.breadcrumb {
  margin-bottom: 5px;
}
</style>