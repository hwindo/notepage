<template>
    <div class="col-12">
        <div class="note" @click.prevent="toggleEditMode">
            <div class="note__time">{{ time | timeFormat }}</div>
            <div class="note__actions">
                <a href="#" class="d-block">
                    <i
                        class="ri-delete-bin-line"
                        @click.stop.prevent="deleteNote"
                    ></i>
                </a>
            </div>
            <div class="note__content">{{ text }}</div>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'nuxt-property-decorator'
import Note from '@/models/Note'

@Component({
    filters: {
        timeFormat(val: string) {
            const date: Date = new Date(val)
            const timeFormat = `${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}`
            return timeFormat
        }
    }
})
export default class NoteView extends Vue {
    editMode: boolean = false
    @Prop() note!: Note
    @Prop() index!: number

    get id(): number {
        return this.note.id
    }

    get text(): string {
        return this.note.text
    }

    get time(): string {
        return this.note.created
    }

    toggleEditMode() {
        // toggle edit mode should be controlled by parent
        this.editMode = !this.editMode
        this.$emit('editNote', this.id)
        console.log('editMode: ', this.editMode)
    }

    deleteNote() {
        this.$emit('deleteNote', this.index)
    }
}
</script>

<style lang="scss">
.note {
    $note: &;
    border: 0.125rem solid lightgray;
    border-radius: 1rem;
    padding: 1.6rem 1rem 1rem;
    margin-bottom: 1rem;
    &:hover {
        cursor: pointer;
        background-color: var(--color-white);
        border-color: var(--gray);
        #{$note}__actions {
            display: block;
        }
    }
    &__actions,
    &__time {
        position: absolute;
    }
    &__actions {
        display: none;
        top: -1.6rem;
        right: 1rem;
        margin-right: 1rem;
        background-color: var(--gray);
        color: var(--color-white);
        padding: 0 1rem;
        border-radius: 0.25rem 0.25rem 0 0;
        a {
            text-decoration: none;
            color: unset;
        }
    }
    &__time {
        top: -0.7rem;
        background-color: var(--color-white);
        padding: 0 0.5rem;
        border-radius: 1rem;
    }
    &__content,
    &__time {
        padding-left: 0.5rem;
    }
}
</style>
