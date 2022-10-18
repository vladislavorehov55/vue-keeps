<template>
  <TopPanel v-if="chosenNotesIdArr.length"
            :chosenNotesCount="chosenNotesIdArr.length"
            @closeTopPanel="closeTopPanel"
            :changeNoteBackgroundColor="changeNoteBackgroundColor"
            :returnFromArchive="returnFromArchive"
            :deleteNote="deleteNote"
            :returnFromTrash="returnFromTrash"
            :deleteForever="deleteForever"
            :addToArchive="addToArchive"
  />
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
             :changeNoteBackgroundColor="changeNoteBackgroundColor"
  />
</template>

<script>
import MyHeader from "@/components/MyHeader";
import LeftPanel from "@/components/LeftPanel";
import NotesList from "@/components/NotesList";
import AddNoteForm from "@/components/AddNoteForm";
import {computed} from "vue";
import TopPanel from "@/components/TopPanel";

export default {
  name: 'App',
  components: {
    TopPanel,
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
      chosenNotesIdArr: []
    }
  },
  provide() {
    return {
      activeLeftPanelListItemInd: computed(() => this.activeLeftPanelListItemInd)
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
    closeTopPanel() {
      this.chosenNotesIdArr = []
      const key = Object.keys(this.notes)[this.activeLeftPanelListItemInd]
      this.notes[key] = this.notes[key].map(note => {
        note.isChosen = false
        return note
      })
    },
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
      if (this.chosenNotesIdArr.includes(id)) {
        this.chosenNotesIdArr.splice(this.chosenNotesIdArr.indexOf(id), 1)
      }
      else {
        this.chosenNotesIdArr.push(id)
      }
      const key = Object.keys(this.notes)[this.activeLeftPanelListItemInd]
      this.notes[key] = this.notes[key].map((note) => {
        if (note.id === id) note.isChosen = !note.isChosen
        return note
      })
    },


    deleteNote(note) {
      const key = Object.keys(this.notes)[Number(this.activeLeftPanelListItemInd)]
      if (this.chosenNotesIdArr.length) {
        const newTrash = [...this.notes.trash]
        this.notes[key] = this.notes[key].filter(note => {
          if (!this.chosenNotesIdArr.includes(note.id)) {
            return note
          }
          const copyNote = {...note}
          copyNote.isChosen = false
          copyNote.type = key
          newTrash.push(copyNote)
        })
        this.notes.trash = newTrash
        this.chosenNotesIdArr = []
      }
      else {
        const copyNote = {...note}
        this.notes[key] = this.notes[key].filter(item => item.id !== copyNote.id)
        this.notes.trash.push(copyNote)
      }
    },
    returnFromTrash(note) {
      if (this.chosenNotesIdArr.length) {
        this.notes.trash = this.notes.trash.filter(item => {
          if (!this.chosenNotesIdArr.includes(item.id)) {
            return true
          }
          const copyNote = {...item}
          copyNote.isChosen = false
          this.notes[copyNote.type].push(copyNote)
          return false
        })
        this.chosenNotesIdArr = []
      }
      else {
        this.notes.trash = this.notes.trash.filter(item => item.id !== note.id)
        this.notes[note.type].push(note)
      }
    },
    deleteForever(id) {
      if (this.chosenNotesIdArr.length) {
        this.notes.trash = this.notes.trash.filter(note => !this.chosenNotesIdArr.includes(note.id))
        this.chosenNotesIdArr = []
      }
      else {
        this.notes.trash = this.notes.trash.filter(note => note.id !== id)
      }
    },
    addToArchive(note, flag) {
      if (this.chosenNotesIdArr.length) {
        this.notes.main = this.notes.main.filter(note => {
          if (!this.chosenNotesIdArr.includes(note.id)) {
            return true
          }
          const copyNote = {...note}
          copyNote.isChosen = false
          copyNote.type = 'archive'
          this.notes.archive.push(copyNote)
          this.chosenNotesIdArr = []
        })
      }
      else {
        const copyNote = {...note}
        if (!flag) {
          this.notes.main = this.notes.main.filter(item => item.id !== copyNote.id)
        }
        copyNote.type = 'archive'
        this.notes.archive.push(copyNote)
      }
    },


    returnFromArchive(note) {
      if (this.chosenNotesIdArr.length) {
        const newArchive = []
        this.notes.archive.forEach(note => {
          const copyNote = {...note}
          if (this.chosenNotesIdArr.includes(note.id)) {
            copyNote.type = 'main'
            copyNote.isChosen = false
            this.notes.main.push(copyNote)
          }
          else {
            newArchive.push(copyNote)
          }
        })
        this.notes.archive = newArchive
      }
      else {
        const copyNote = {...note}
        this.notes.archive = this.notes.archive.filter(note => note.id !== copyNote.id)
        copyNote.type = 'main'
        this.notes.main.push(copyNote)
      }
      this.chosenNotesIdArr = []

    },
    changeNoteBackgroundColor(id, color) {
      const key = Object.keys(this.notes)[this.activeLeftPanelListItemInd]
      this.notes[key] = this.notes[key].map(note => {
        if (this.chosenNotesIdArr.length) {
          if (this.chosenNotesIdArr.includes(note.id)) {
            note.backgroundColor = color
          }
        }
        else {
          if (note.id === id) {
            note.backgroundColor = color
          }
        }
        return note
      })
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
