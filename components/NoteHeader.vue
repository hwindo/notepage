<template>
    <header class="header text-center my-4">
        <a href="#" class="header__text-link">
            <h1>Meeting With Development Team</h1>
        </a>
        <div class="header__time mb-4">{{ date }}</div>
    </header>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'

@Component
export default class NoteHeader extends Vue {
    date: string = new Date().toLocaleString()
}
</script>

<style lang="scss">
.header {
    border-bottom: 4px solid lightgray;
    animation: slidedown 0.618s cubic-bezier(0.075, 0.82, 0.165, 1) forwards;
    &__text-link {
        display: block;
        color: unset;
        &:hover {
            text-decoration: none;
        }
    }
}
</style>
