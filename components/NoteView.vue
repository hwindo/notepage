<template>
    <div class="col-12">
        <div class="note">
            <div class="note__time">{{ time | timeFormat }}</div>
            <div class="note__actions">
                <a href="#" class="d-block">
                    <i
                        class="ri-delete-bin-line"
                        @click.stop.prevent="deleteNote"
                    ></i>
                </a>
            </div>
            <template v-if="editMode">
                <editor-menu-bar
                    :editor="editor"
                    v-slot="{ commands, isActive }"
                >
                    <div class="menubar">
                        <button
                            class="menubar__button"
                            :class="{ 'is-active': isActive.bold() }"
                            @click="commands.bold"
                        >
                            <i class="ri-bold"></i>
                        </button>

                        <button
                            class="menubar__button"
                            :class="{ 'is-active': isActive.italic() }"
                            @click="commands.italic"
                        >
                            <i class="ri-italic"></i>
                        </button>

                        <button
                            class="menubar__button"
                            :class="{ 'is-active': isActive.underline() }"
                            @click="commands.underline"
                        >
                            <i class="ri-underline"></i>
                        </button>

                        <button
                            class="menubar__button"
                            :class="{ 'is-active': isActive.strike() }"
                            @click="commands.strike"
                        >
                            <i class="ri-strikethrough"></i>
                        </button>

                        <button
                            class="menubar__button"
                            :class="{ 'is-active': isActive.paragraph() }"
                            @click="commands.paragraph"
                        >
                            <i class="ri-paragraph"></i>
                        </button>

                        <button
                            class="menubar__button"
                            :class="{
                                'is-active': isActive.heading({ level: 1 })
                            }"
                            @click="commands.heading({ level: 1 })"
                        >
                            <i class="ri-h-1"></i>
                        </button>

                        <button
                            class="menubar__button"
                            :class="{ 'is-active': isActive.bullet_list() }"
                            @click="commands.bullet_list"
                        >
                            <i class="ri-list-unordered"></i>
                        </button>

                        <button
                            class="menubar__button"
                            :class="{ 'is-active': isActive.ordered_list() }"
                            @click="commands.ordered_list"
                        >
                            <i class="ri-list-ordered"></i>
                        </button>

                        <button class="menubar__button" @click="updateNote">
                            <i class="ri-save-line"></i>
                        </button>

                        <button class="menubar__button" @click="cancelEdit">
                            <i class="ri-close-line"></i>
                        </button>
                    </div>
                </editor-menu-bar>
                <editor-content :editor="editor"></editor-content>
            </template>
            <div
                v-else
                v-html="text"
                class="note__content"
                @click.prevent="toggleEditMode"
            ></div>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'nuxt-property-decorator'
import { Editor, EditorContent, EditorMenuBar } from 'tiptap'
import {
    Blockquote,
    CodeBlock,
    HardBreak,
    Heading,
    HorizontalRule,
    OrderedList,
    BulletList,
    ListItem,
    TodoItem,
    TodoList,
    Bold,
    Code,
    Italic,
    Link,
    Strike,
    Underline,
    History
} from 'tiptap-extensions'
import Note from '@/models/Note'

@Component({
    components: {
        EditorContent,
        EditorMenuBar
    },
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
    temporaryText: string
    editor: Editor = new Editor({
        content: this.text,
        extensions: [
            new Blockquote(),
            new BulletList(),
            new CodeBlock(),
            new HardBreak(),
            new Heading({ levels: [1, 2, 3] }),
            new HorizontalRule(),
            new ListItem(),
            new OrderedList(),
            new TodoItem(),
            new TodoList(),
            new Link(),
            new Bold(),
            new Code(),
            new Italic(),
            new Strike(),
            new Underline(),
            new History()
        ]
    })

    @Prop() note!: Note
    @Prop() index!: number

    get id(): number {
        return this.note.id
    }

    get text(): string {
        return this.note.text
    }

    set text(value) {
        this.note.text = value
    }

    get time(): string {
        return this.note.created
    }

    mounted() {
        this.editor.on('update', ({ getHTML }) => {
            // get new content on update
            this.temporaryText = getHTML()
        })
    }

    beforeDestroy() {
        this.editor.destroy()
    }

    toggleEditMode() {
        // toggle edit mode should be controlled by parent
        this.editor.content = this.text
        this.editMode = !this.editMode
        this.$emit('editNote', this.id)
        console.log('editMode: ', this.editMode)
    }

    deleteNote() {
        this.$emit('deleteNote', this.index)
    }

    updateNote() {
        this.text = this.temporaryText
        this.$emit('updateNote', {
            index: this.index,
            note: this.note
        })
        this.editMode = !this.editMode
    }

    cancelEdit() {
        this.editMode = !this.editMode
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
    animation-delay: 0.618s;
    animation: slideup 0.382s ease-out forwards;
    &:hover {
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
    &__text {
        cursor: pointer;
    }
}

.menubar {
}

.proseMirror {
    &:hover {
        cursor: text;
    }
}
</style>
