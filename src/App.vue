<template>
  <MyHeader :notesDisplay="notesDisplay"
            @setLeftPanelVisability="setLeftPanelVisability"
            :activeLeftPanelListItemInd="activeLeftPanelListItemInd"
            @changeNotesDisplay="changeNotesDisplay"
  />
  <LeftPanel :isOpenLeftPanel="isOpenLeftPanel"
             :activeLeftPanelListItemInd="activeLeftPanelListItemInd"
             @setActiveLeftPanelListItemInd="setActiveLeftPanelListItemInd"
  />
  <AddNoteForm v-if="activeLeftPanelListItemInd === 0"
               @createNote="createNote"
               @addToArchive="addToArchive"
  />
  <NotesList :notes="getNotes"
             @chooseNote="chooseNote"
             :notesDisplay="notesDisplay"
             @deleteNote="deleteNote"
             @returnFromTrash="returnFromTrash"
             @deleteForever="deleteForever"
             @addToArchive="addToArchive"
             @returnFromArchive="returnFromArchive"
             :activeLeftPanelListItemInd="activeLeftPanelListItemInd"

  />
</template>

<script>
import MyHeader from "@/components/MyHeader";
import LeftPanel from "@/components/LeftPanel";
import NotesList from "@/components/NotesList";
import AddNoteForm from "@/components/AddNoteForm";

export default {
  name: 'App',
  components: {
    AddNoteForm,
    NotesList,
    MyHeader,
    LeftPanel
  },
  data() {
    return {
      notesDisplay: 'list',
      isOpenLeftPanel: false,
      activeLeftPanelListItemInd: 0,
      notes: {
        main: [
          {
            id: 0,
            title: '1',
            text: '2',
            backgroundColor: 'White',
            isChosen: false,
            type: 'main',
            imgList: []
          }
        ],
        archive: [],
        trash: []
      },
    }
  },
  computed: {
    getNotes() {
      switch (this.activeLeftPanelListItemInd) {
        case 0 || null:
          return this.notes.main
        case 1:
          return this.notes.archive
        case 2:
          return this.notes.trash
        default:
          return this.notes.main
      }
    }
  },
  methods: {
    setLeftPanelVisability() {
      this.isOpenLeftPanel = !this.isOpenLeftPanel
    },
    setActiveLeftPanelListItemInd(ind) {
      this.activeLeftPanelListItemInd = ind
    },
    changeNotesDisplay(notesDisplayType) {
      this.notesDisplay = notesDisplayType
    },
    createNote(title, text, backgroundColor, imgList) {
      this.notes.main.push(
          {
            title, text, id: Date.now(), backgroundColor, isChosen: false, type: 'main',
            imgList
          }
      )
    },
    chooseNote(id) {
      this.notes.main = this.notes.main.map((note) => {
        if (note.id === id) note.isChosen = !note.isChosen
        return note
      })
    },
    deleteNote(note) {
      const copyNote = {...note}
      const key = Object.keys(this.notes)[Number(this.activeLeftPanelListItemInd)]
      this.notes[key] = this.notes[key].filter(item => item.id !== copyNote.id)
      this.notes.trash.push(copyNote)
    },
    returnFromTrash(note) {
      this.notes.trash = this.notes.trash.filter(item => item.id !== note.id)
      this.notes[note.type].push(note)
    },
    deleteForever(id) {
      this.notes.trash = this.notes.trash.filter(note => note.id !== id)
    },
    addToArchive(note, flag) {
      const copyNote = {...note}
      if (!flag) {
        this.notes.main = this.notes.main.filter(item => item.id !== copyNote.id)
      }
      copyNote.type = 'archive'
      this.notes.archive.push(copyNote)
    },
    returnFromArchive(note) {
      const copyNote = {...note}
      this.notes.archive = this.notes.archive.filter(note => note.id !== copyNote.id)
      copyNote.type = 'main'
      this.notes.main.push(copyNote)
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

#app {
  min-height: 100vh;
}

.backgroundColorWhite {
  background: white;
}

.backgroundColorGrey {
  background: grey;
}

.backgroundColorBlue {
  background: blue;
}

.backgroundColorRed {
  background: red;
}

.backgroundColorGreen {
  background: green;
}

.backgroundColorYellow {
  background: yellow;
}

.backgroundColorPink {
  background: pink;
}

.backgroundColorOrange {
  background: orange;
}

.backgroundColorBrown {
  background: brown;
}

</style>
