<template>
    <div class="note">
        <div class="note__left-side">
            <div class="note__input_name">
                <input v-model="note.title" placeholder="Название заметки" @:keyup.ctrl.90="stop" @:keyup.shift.ctrl.88="stop">
            </div>
            <div class="note__tasks">
                <ul v-for="(task, index) in note.tasks" v-bind:key="task.id">
                    <li v-bind:class="{ note__task_done: task.done }">
                        <input type="checkbox" @click="checkDone(index)" v-model="task.done" :key="index">
                        <input v-model="task.text">
                        <span class="delete_span hairline" @click="deleteTask(index)"></span>
                    </li>
                </ul>
            </div>
            <div class="note__input">
                <textarea v-model="text" placeholder="Впишите задание и нажмите enter"
                          @keyup.enter="createTask"></textarea>
            </div>
        </div>
        <Modal :note="note" v-bind:type="alert_type" v-if='alert' @confirm="deleteNote('default')" @cancel="cancelDelete" @confirmCancel="onConfirmCancel('default')"/>
        <div class="note__right-side">
            <div>
                <button class="btn" @click="openModal('cancel')">Отменить</button>
                <button class="btn btn_red" @click="openModal('delete')">Удалить</button>
            </div>
            <div>
                <button class="btn btn_green" @click="saveNote('default')">Сохранить</button>
            </div>
        </div>
    </div>
</template>

<script>

    import Modal from '../Modal'

    export default {
        name: "EditeNote",

        props: {
            note: {
                type: Object
            }
        },
        data() {
            return {
                text: '',
                edited_tasks: [],
                checked: false,
                alert: false,
                const_title: '',
                alert_type: ''
            }
        },
        methods: {
            deleteTask(index) {
                this.$delete(this.note.tasks, index)
            },
            checkDone(index) {
                if (~index) {
                    let task = this.note.tasks[index];
                    if (task.done) {
                        task.done = false;
                        console.log(task.done);

                    } else {
                        task.done = true;
                        console.log(task.done);
                    }
                }
            },
            createTask() {
                this.note.tasks.push(
                    {text: this.text, done: false}
                );
                console.log(this.note.tasks);
                this.text = '';
            },
            saveNote(adding) {
                if (this.edited_tasks.length > 0) {
                    this.note.tasks = this.edited_tasks;
                }
                console.log(this.note);
                this.$emit('editNote', {
                    note: this.note,
                    adding: adding,
                })
            },
            openModal(alert_type) {
                if (alert_type === 'cancel'){
                    this.alert_type = 'cancel'
                }if (alert_type === 'delete'){
                    this.alert_type = 'delete'
                }
                this.alert = true;
            },
            cancelDelete() {
                this.alert = false;
            },
            deleteNote(adding){
                this.$emit('confirm', {
                    note: this.note,
                    adding: adding,
                })
            },
            onConfirmCancel(adding){
                this.$emit('cancel', {
                    note: this.note,
                    adding: adding,
                })
            }
        },
        components: {
            Modal
        },
    }
</script>
