<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          
          <message v-if="message" :message="message" />

          <newNote :note="note" @addNote="addNote" />

          <div class="note-header">
            <h1>{{ title }}</h1>

            <search @search="search = $event" :value="search" />

            <div class="icons">
              <svg :class="{ active: grid }" @click="grid = true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
              <svg :class="{ active: !grid }" @click="grid = false" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
            </div>
          </div>

          <notes @remove="removeNote" :notes="notesFilter" :grid="grid" />

        </div>
      </section>
    </div>
  </div>
</template>

<script>
import message from '@/components/Message.vue';
import newNote from './components/NewNote.vue';
import notes from './components/Notes.vue';
import search from './components/Search.vue';
  export default {
    components: {
      message,
      newNote,
      notes,
      search
    },
    data() {
      return {
        title: 'Notes App',
        search: '',
        message: null,
        grid: true,
        note: {
          title: '',
          desc: '',
          priority: 'Standard'
        },
        notes: [
          {
            title: 'First Note',
            desc: 'Description for first note',
            date: new Date(Date.now()).toLocaleString(),
            isEditingTitle: false,
            isEditingDesc: false
          },
          {
            title: 'Second Note',
            desc: 'Description for second note',
            date: new Date(Date.now()).toLocaleString(),
            isEditingTitle: false,
            isEditingDesc: false
          },
          {
            title: 'Third Note',
            desc: 'Description for third note',
            date: new Date(Date.now()).toLocaleString(),
            isEditingTitle: false,
            isEditingDesc: false
          }
        ]
      }
    },
    computed: {
      notesFilter() {
        let array = this.notes,
            search = this.search

        if(!search) return array
        search = search.trim().toLowerCase()
        array = array.filter(item => {
          if(item.title.toLowerCase().indexOf(search) !== -1) return item
        })
        return array
        // return this.notes.filter(note => note.title.toLowerCase().includes(this.search.toLowerCase()))
      }
    },
    methods: {
      addNote() {
        // console.log(this.note)
        const { title, desc, priority } = this.note

        if(title.trim() === '' || desc.trim() === '') {
          this.message = 'title can`t be empty'
          return
        } 

        this.notes.push({
          title: title,
          desc: desc,
          priority: priority || 'Standard',
          date: new Date(Date.now()).toLocaleString()
        })
        this.note.title = '',
        this.note.desc = '',
        this.note.priority = 'Standard'
        this.message = null
      },
      removeNote(index) {
        // this.notes.splice(index, 1)
        const noteToRemove = this.notesFilter[index];
        const originalIndex = this.notes.findIndex(note => 
          note.title === noteToRemove.title &&
          note.desc === noteToRemove.desc &&
          note.date === noteToRemove.date &&
          note.priority === noteToRemove.priority
        );

        if (originalIndex !== -1) {
          this.notes.splice(originalIndex, 1);
        }
      }
    },
  }
</script>
