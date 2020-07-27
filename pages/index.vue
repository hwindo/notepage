<template>
    <div class="container-fluid">
        <div class="row">
            <div
                class="full-h col d-flex flex-column justify-content-center align-items-center"
            >
                <h1 class="title text-center">
                    NotePage
                </h1>
                <h2 class="subtitle text-center">
                    A Note Taking Web Application
                </h2>
                <div class="links text-center">
                    <nuxt-link to="/note/new" class="button--green">
                        Create Note Page
                    </nuxt-link>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'
import NoteHeader from '~/components/NoteHeader.vue'

export default Vue.extend({
    components: {
        NoteHeader
    }
})
</script>

<style>
.container {
    margin: 0 auto;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
}

.title {
    font-family: 'Quicksand', 'Source Sans Pro', -apple-system,
        BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial,
        sans-serif;
    display: block;
    font-weight: 300;
    font-size: 100px;
    color: #35495e;
    letter-spacing: 1px;
}

.subtitle {
    font-weight: 300;
    font-size: 42px;
    color: #526488;
    word-spacing: 5px;
    padding-bottom: 15px;
}

.links {
    padding-top: 15px;
}
</style>
