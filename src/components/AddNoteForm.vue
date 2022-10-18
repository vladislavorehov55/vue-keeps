<template>
  <form @submit.prevent
        class="form"
        :class="{['backgroundColor' +  backgroundColor]: true}"
        ref="form"
        @click="isOpened = true">

    <MyImg :imgList="imgList" @deletePicture="deletePicture"/>

    <textarea :style="{height: titleHeight}"
              placeholder="Введите заголовок"
              class="textarea"
              name='title'
              :value="title"
              @input="inputHandler"
              v-if="isOpened"
    />
    <textarea :style="{height: textHeight}"
              placeholder="Заметки"
              class="textarea"
              name='text'
              :value="text"
              @input="inputHandler"
    />
    <div class="wrap" v-if="isOpened">
      <MyMenu v-model:backgroundColor="backgroundColor"
              :activeLeftPanelListItemInd="0"
              @choosePic="choosePic"
              :inAddForm="true"
              @addToArchive="addToArchive"
      />
      <MyButton @click="closeForm">Закрыть</MyButton>
    </div>
  </form>
</template>

<script>
import MyMenu from "@/components/MyMenu";
import MyButton from "@/components/general/MyButton";
import MyImg from "@/components/general/MyImg";

export default {
  name: "AddNoteForm",
  components: {MyImg, MyButton, MyMenu},
  data()  {
    return {
      isOpened: false,
      title: '',
      text: '',
      titleHeight: '',
      textHeight: '',
      backgroundColor: 'White',
      imgList: []
    }
  },
  methods: {
    inputHandler(e) {
      if (e.target.name === 'title') {
        this.title = e.target.value
        this.titleHeight = e.target.scrollHeight + 'px'
      }
      else {
        this.text = e.target.value
        this.textHeight = e.target.scrollHeight + 'px'
      }
    },
    closeForm(e) {
      e.stopPropagation()
      if (this.text || this.title || this.imgList.length) {
        this.$emit('createNote', this.title, this.text, this.backgroundColor, this.imgList)
      }
      this.text = ''
      this.textHeight = ''
      this.title= ''
      this.titleHeight = ''
      this.backgroundColor = 'White'
      this.isOpened = false
      this.imgList = []
    },
    choosePic(e) {
      const srcList = this.imgList;
      for (let file of e.target.files) {
        const reader = new FileReader()
        reader.readAsDataURL(file)
        reader.onload = () => {
          srcList.push({id: Date.now(), src: reader.result})
          if (srcList.length === e.target.files.length) {
            this.imgList = srcList
          }
        }
      }
    },
    deletePicture(id) {
      this.imgList = this.imgList.filter(item => item.id !== id)
    },
    addToArchive() {
      if (!this.text && !this.title && !this.imgList.length) {
        return
      }
      const note = {title: this.title, text: this.text, backgroundColor: this.backgroundColor, imgList: this.imgList}
      this.text = ''
      this.textHeight = ''
      this.title= ''
      this.titleHeight = ''
      this.backgroundColor = 'White'
      this.isOpened = false
      this.imgList = []
      this.$emit('addToArchive', note, true)
    }
  },
  mounted() {
    document.addEventListener('click', (e) => {
      if (this.isOpened && !this.$refs.form.contains(e.target)){
        this.closeForm(e)
      }
    })
  }
}
</script>

<style scoped>
.form {
  border-radius: 5px;
  max-width: 500px;
  margin: 10px auto;
  min-height: 50px;
  padding: 10px;
  box-shadow: 0 3px 5px rgb(0 0 0 / 20%);
}
.textarea {
  width: 100%;
  border: none;
  padding: 0;
  border-radius: 5px;
  margin-bottom: 10px;
  outline: none;
  resize: none;
  font-size: 15px;
  height: 20px;
  background: transparent;
}
.wrap {
  display: flex;
  justify-content: space-between;
}


</style>