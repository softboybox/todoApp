<template>
    <div class="note">
        <div class="note__left-side">
            <div class="note__input_name">
                <input v-model="title" placeholder="Название заметки">
            </div>
            <div class="note__tasks">
                <ul v-for="task in tasks" v-bind:key="task.id">
                    <li>
                       - {{task.text}}
                    </li>
                </ul>
            </div>
            <div class="note__input">
                <textarea v-model="text" placeholder="Впишите задание и нажмите enter" @keyup.enter="createTask"></textarea>
            </div>
        </div>
        <div class="note__right-side">
            <div>
                <button class="btn btn_green" @click="createNote">Сохранить</button>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Editing",
        data() {
            return {
                adding: 'default',
                title: '',
                text: '',
                tasks: []
            }
        },
        methods: {
            createTask() {
                this.tasks.push(
                    {text: this.text, done: false}
                );
                this.text = '';
            },
            createNote() {
                this.$emit('newNote', {
                    note:
                        {
                            title: this.title,
                            tasks: this.tasks
                        }
                    ,
                    adding: 'default'
                })
            },
            cancelCreate() {
                this.$emit('newNote', {
                    adding: 'default'
                })
            }
        }
    }
</script>

<style scoped>

</style>