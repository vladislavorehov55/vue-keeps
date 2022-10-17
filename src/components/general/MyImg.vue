<template v-if="imgList.length">
  <div v-for="img in imgList" :key="img.id" class="imgWrap">
    <img :key="img.id"
         :src="img.src"
         class="img"
    />
    <MyButton class="deleteImgWrap" @click.stop="$emit('deletePicture', img.id)">
      <img class="img" src="data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjE4cHgiIHdpZHRoPSIxOHB4IiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgZmlsbD0iI0ZGRkZGRiI+CiA8cGF0aCBkPSJtMTIgMzhjMCAyLjIxIDEuNzkgNCA0IDRoMTZjMi4yMSAwIDQtMS43OSA0LTR2LTI0aC0yNHYyNHptMjYtMzBoLTdsLTItMmgtMTBsLTIgMmgtN3Y0aDI4di00eiIvPgogPHBhdGggZD0ibTAgMGg0OHY0OGgtNDh6IiBmaWxsPSJub25lIi8+Cjwvc3ZnPgo=" alt="">
    </MyButton>
  </div>
</template>


<script>
export default {
  name: "MyImg",
  props: {
    imgList: Array
  }
}
</script>

<style scoped>
.imgWrap {
  width: 100%;
  margin-bottom: 10px;
  position: relative;
}
.img {
  width: 100%;
}
.deleteImgWrap {
  background: black!important;
  width: 30px;
  height: 30px;
  position: absolute;
  bottom: 5px;
  right: 5px;
  display: none;

}
.imgWrap:hover .deleteImgWrap {
  display: block;
}
</style>