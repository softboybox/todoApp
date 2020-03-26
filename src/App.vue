<!--

 App structure:

 App.vue /
    Home.vue ( main page ) /
       Modal.vue
       Note.vue
       EditNote.vue ( edit page ) /
           Modal.vue
       CreateNote.uve ( create page )
 -->


<template>
    <div id="app">
        <Home/>
    </div>
</template>

<script>
    import Home from './components/pages/Home.vue'

    export default {
        name: 'App',
        components: {
            Home
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #1F0318;
        margin: 5% 10% 0;
        padding: 1em;

    }
</style>
<style>
    @import "../public/css/app.95124464.css";
</style>