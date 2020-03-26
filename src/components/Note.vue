<template>
    <div class="note">
        <div class="note__left-side">
            <div class="note__name">
                <h3>{{note.title}}</h3>
            </div>
            <div class="note__tasks">
                <ul v-for="task in note.tasks" v-bind:key="task.id">
                    <li v-bind:class="{ note__task_done: task.done }"> - {{task.text}}</li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        props: {
            note: {
                type: Object
            }
        },

        name: "Note",

    }

</script>
