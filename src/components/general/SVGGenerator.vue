<template>
  <svg v-if="name === 'circle'" class="colorSubmenuSvg">
    <circle r="12"
            v-for="(circle, ind) in circles"
            :cx="circle.cx" :cy="circle.cy" :fill="circle.fill"
            :stroke="circle.stroke"
            :key="ind"
            @click="$emit('updateBackgroundColorHandler', circle.fill[0].toUpperCase() + circle.fill.slice(1))"
    />
  </svg>


  <svg width='24px' height='24px' viewBox='0 0 24 24' v-else>
    <path v-if="name === 'home-note'"
        d='M9 21c0 .55.45 1 1 1h4c.55 0 1-.45 1-1v-1H9v1zm3-19C8.14 2 5 5.14 5 9c0 2.38 1.19 4.47 3 5.74V17c0 .55.45 1 1 1h6c.55 0 1-.45 1-1v-2.26c1.81-1.27 3-3.36 3-5.74 0-3.86-3.14-7-7-7zm2.85 11.1l-.85.6V16h-4v-2.3l-.85-.6A4.997 4.997 0 0 1 7 9c0-2.76 2.24-5 5-5s5 2.24 5 5c0 1.63-.8 3.16-2.15 4.1z'
    />
    <path v-else-if="name === 'trash-note'"
        d="M15 4V3H9v1H4v2h1v13c0 1.1.9 2 2 2h10c1.1 0 2-.9 2-2V6h1V4h-5zm2 15H7V6h10v13z"
    />
    <path v-else-if="name === 'archive-note'"
        d="M20.54 5.23l-1.39-1.68C18.88 3.21 18.47 3 18 3H6c-.47 0-.88.21-1.16.55L3.46 5.23C3.17 5.57 3 6.02 3 6.5V19c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V6.5c0-.48-.17-.93-.46-1.27zM6.24 5h11.52l.83 1H5.42l.82-1zM5 19V8h14v11H5zm11-5.5l-4 4-4-4 1.41-1.41L11 13.67V10h2v3.67l1.59-1.59L16 13.5z"
    />
    <path v-else-if="name === 'menu'"
        d="M3 18h18v-2H3v2zm0-5h18v-2H3v2zm0-7v2h18V6H3z"
    />
    <path v-else-if="name === 'search'"
          d='M20.49,19l-5.73-5.73C15.53,12.2,16,10.91,16,9.5C16,5.91,13.09,3,9.5,3S3,5.91,3,9.5C3,13.09,5.91,16,9.5,16 c1.41,0,2.7-0.47,3.77-1.24L19,20.49L20.49,19z M5,9.5C5,7.01,7.01,5,9.5,5S14,7.01,14,9.5S11.99,14,9.5,14S5,11.99,5,9.5z'
    />
    <path v-else-if="name === 'close'"
          d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"
    />
  </svg>
</template>

<script>
import {circles} from "@/data";

export default {
  name: "SVGGenerator",
  props: {
    name: String,
    modelBackgroundColor: String
  },
  data() {
    return {
      circles: circles
    }
  }
}
</script>

<style scoped>
.colorSubmenuSvg {
  width: 80px;
  height: 80px;
}
</style>