<template>
    <div class="container-fluid page">
        <div class="row">
            <div class="col">
                <note-header></note-header>
            </div>
        </div>

        <div class="row">
            <!-- content -->
            <template v-for="(note, index) in notes">
                <note-view
                    :id="'note-' + note.id"
                    :key="note.id"
                    :index="index"
                    :note="note"
                    @deleteNote="deleteNote"
                ></note-view>
            </template>
        </div>

        <div class="input-note">
            <b-input-group>
                <b-form-input
                    @keyup.enter="handleInputSubmit"
                    v-model="inputValue"
                ></b-form-input>
                <b-input-group-append>
                    <b-button
                        class="d-flex align-items-center"
                        @click="handleInputSubmit"
                        variant="info"
                    >
                        Create Note
                        <i class="ri-arrow-up-line ml-2"></i>
                    </b-button>
                </b-input-group-append>
            </b-input-group>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'
import NoteHeader from '@/components/NoteHeader'
import NoteView from '@/components/NoteView'
import Note from '@/models/Note'

@Component({
    components: {
        NoteHeader,
        NoteView
    }
})
export default class New extends Vue {
    inputValue: string = ''
    notes: Array<Note> = [
        {
            id: 1,
            created: '2020-06-13T08:18:14.388Z',
            text:
                "Meeting notes are exactly what the name implies––notes. They're quick references to ideas, goals, deadlines, data, and anything else important that's covered in your meeting. Minutes, however, are more formal and often include: A list of everyone who attended the meeting. An absentee list"
        }
    ]

    handleInputSubmit(): void {
        console.log('clicked', this.inputValue)
        this.addNote()
        this.$nextTick(() => {
            this.inputValue = ''
        })
    }

    addNote() {
        if (!this.inputValue) {
            console.log('please fill up input')
            return
        }
        const newNote: Note = {
            id: this.notes.length + 1,
            created: new Date().toISOString(),
            text: this.inputValue
        }
        this.notes.push(newNote)
        this.$nextTick(() => {
            const newNoteId = '#note-' + newNote.id
            console.log(newNoteId)
            document.querySelector(newNoteId)!.scrollIntoView({
                behavior: 'smooth'
            })
        })
    }

    deleteNote(index: number) {
        console.log('delete note', index, this.notes[index])
        this.notes.splice(index, 1)
    }
}
</script>

<style lang="scss">
.page {
    padding-bottom: 50px; // compensate input
}
.input-note {
    position: fixed;
    bottom: 0;
    width: 90%;
    width: calc(100% - 2rem);
    margin-bottom: 1rem;
}
</style>
