<template>
  <ul class="wrapper"
      :class="notesDisplay === 'list' ? 'listStyle' : 'gridStyle'"
  >
    <NoteItem v-for="note in notes"
              :key="note.id"
              :note="note"
              @chooseNote="$emit('chooseNote', note.id)"
              :notesDisplay="notesDisplay"
              @deleteNote="$emit('deleteNote', note)"
              @returnFromTrash="$emit('returnFromTrash', note)"
              @deleteForever="$emit('deleteForever', note.id)"
              @addToArchive="$emit('addToArchive', note)"
              @returnFromArchive="$emit('returnFromArchive', note)"
              :changeNoteBackgroundColor="changeNoteBackgroundColor"
    />
  </ul>
</template>

<script>
import NoteItem from "@/components/NoteItem";
export default {
  name: "NotesList",
  components: {NoteItem},
  props: {
    notes: Array,
    notesDisplay: String,
    changeNoteBackgroundColor: Function
  },
}
</script>

<style scoped>
.wrapper {
  list-style: none;
  padding: 20px 10px;
}
.gridStyle {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: flex-start;
  justify-content: space-between;
}
</style>