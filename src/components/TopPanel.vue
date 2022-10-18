<template>
  <div :class="$style.wrap">
    <div :class="$style.leftGroup">
      <MyButton @click="$emit('closeTopPanel')">
        <img src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSIjMDAwMDAwIj4KICA8cGF0aCBkPSJNMTkgNi40MUwxNy41OSA1IDEyIDEwLjU5IDYuNDEgNSA1IDYuNDEgMTAuNTkgMTIgNSAxNy41OSA2LjQxIDE5IDEyIDEzLjQxIDE3LjU5IDE5IDE5IDE3LjU5IDEzLjQxIDEyIDE5IDYuNDF6Ii8+Cjwvc3ZnPgo=' alt=""/>
      </MyButton>
      Выбрано {{chosenNotesCount}}
    </div>
    <div :class="$style.rightGroup">
      <MyMenu :inTopPanel="true"
              :changeNoteBackgroundColor="changeNoteBackgroundColor"
              @returnFromArchive="returnFromArchive"
              @deleteNote="deleteNote"
              @returnFromTrash="returnFromTrash"
              @deleteForever="deleteForever"
              @addToArchive="addToArchive"
      />
    </div>

  </div>
</template>

<script>
import MyButton from "@/components/general/MyButton";
import MyMenu from "@/components/MyMenu";
export default {
  name: "TopPanel",
  components: {MyMenu, MyButton},
  props: {
    chosenNotesCount: Number,
    changeNoteBackgroundColor: Function,
    returnFromArchive: Function,
    deleteNote: Function,
    returnFromTrash: Function,
    deleteForever: Function,
    addToArchive: Function
  }
}
</script>

<style module>
.wrap {
  position: fixed;
  top: 0;
  width: 100%;
  display: flex;
  align-items: center;
  height: 75px;
  border-bottom: 1px solid black;
  justify-content: space-between;
  padding: 5px;
  background: #fff;
}
.leftGroup {
  display: flex;
  align-items: center;
}
.rightGroup {
  width: 25%;
}

</style>