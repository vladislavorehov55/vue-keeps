<template>
  <li class="noteItem__wrapper"
      :class="[{chosen: note.isChosen, ['backgroundColor' + note.backgroundColor]: true}, notesDisplay === 'list' ? 'noteItem__listStyle' : 'noteItem__gridStyle' ]">
    <MyImg :imgList="note.imgList"/>
    <h2 class="title">{{note.title}}</h2>
    <p class="text">{{note.text}}</p>
    <img alt="choose"
         title='Выбрать заметку'
         @click="$emit('chooseNote')"
         class="iconChoose"
         src='data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjRweCIgaGVpZ2h0PSIyNHB4IiB2aWV3Qm94PSIwIDAgMjQgMjQiIHZlcnNpb249IjEuMSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB4bWxuczp4bGluaz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayI+CiAgICA8ZGVmcz4KICAgICAgICA8cGF0aCBkPSJNMTIsMiBDMTcuNTIsMiAyMiw2LjQ4IDIyLDEyIEMyMiwxNy41MiAxNy41MiwyMiAxMiwyMiBDNi40OCwyMiAyLDE3LjUyIDIsMTIgQzIsNi40OCA2LjQ4LDIgMTIsMiBaIE0xMCwxNC4yIEw3LjQsMTEuNiBMNiwxMyBMMTAsMTcgTDE4LDkgTDE2LjYsNy42IEwxMCwxNC4yIFoiIGlkPSJwYXRoLTEiPjwvcGF0aD4KICAgIDwvZGVmcz4KICAgIDxnIGlkPSJjaGVja19jaXJjbGVfMjRweCIgc3Ryb2tlPSJub25lIiBzdHJva2Utd2lkdGg9IjEiIGZpbGw9Im5vbmUiIGZpbGwtcnVsZT0iZXZlbm9kZCI+CiAgICAgICAgPHBvbHlnb24gaWQ9ImJvdW5kcyIgcG9pbnRzPSIwIDAgMjQgMCAyNCAyNCAwIDI0Ij48L3BvbHlnb24+CiAgICAgICAgPG1hc2sgaWQ9Im1hc2stMiIgZmlsbD0id2hpdGUiPgogICAgICAgICAgICA8dXNlIHhsaW5rOmhyZWY9IiNwYXRoLTEiPjwvdXNlPgogICAgICAgIDwvbWFzaz4KICAgICAgICA8dXNlIGlkPSJNYXNrIiBmaWxsPSIjMDAwMDAwIiBmaWxsLXJ1bGU9Im5vbnplcm8iIHhsaW5rOmhyZWY9IiNwYXRoLTEiPjwvdXNlPgogICAgPC9nPgo8L3N2Zz4K'
    />
    <MyMenu @deleteNote="$emit('deleteNote')"
            @returnFromTrash="$emit('returnFromTrash')"
            @deleteForever="$emit('deleteForever')"
            @addToArchive="$emit('addToArchive')"
            @returnFromArchive="$emit('returnFromArchive')"
            :activeLeftPanelListItemInd="activeLeftPanelListItemInd"
            class="noteItem__menu"
    />
  </li>
</template>

<script>
import {circles} from "@/data";
import MyMenu from "@/components/MyMenu";
import MyImg from "@/components/general/MyImg";

export default {
  name: "NoteItem",
  components: {
    MyImg,
    MyMenu

  },
  props: {
    note: Object,
    notesDisplay: String,
    activeLeftPanelListItemInd: Number || null
  },
  data(){
    return {
      circles: circles
    }
  },
  methods: {
    chooseNote() {
      this.isChosen = !this.isChosen
    }
  }
}
</script>

<style scoped>
.noteItem__wrapper {
  border-radius: 5px;
  border: 1px solid #e0e0e0;
  padding: 10px!important;
  position: relative;
}
.iconChoose {
  display: none;
  position: absolute;
  top: -8px;
  left: -10px;
  cursor: pointer;
}
.noteItem__wrapper:hover .iconChoose {
  display: block;
}
.text {
  padding-top: 10px;
}
.chosen {
  border: 2px solid black;
}
.noteItem__listStyle {
  margin: 0 auto 10px;
  max-width: 500px;
}
.noteItem__gridStyle {
  width: 200px;
  margin-bottom: 10px;
}
.noteItem__menu {
  margin-top: 5px;
}

</style>