<!-- Main page -->
<template>
    <div>
        <div v-if="adding === 'default'">
            <div v-if="notes.length === 0">
                <div class="greetings">
                    <p>У вас еще нет ни одной заметки :( <br>Добавьте же скорее!</p>
                </div>
            </div>
            <!-- List of notes -->
            <div v-for="note in notes" v-bind:key="note.id" class="home">
                <div class="home__note">
                    <Note v-bind:note="note"/>
                    <hr>
                </div>
                <div class="home__buttons">
                    <button class="btn" @click="confirmEdit('edit', note)">Изменить</button>
                    <span class="delete_span hairline" @click="openModel(note, 'delete')"></span>
                </div>
                <!-- Modal ( delete note ) -->
                <Modal :note="selectedNote" v-bind:type="alert_type" v-if='alert' @confirm='deleteNote'
                       @cancel="cancelDelete"/>
                <!-- Modal ( delete note ) | end -->
            </div>
            <!-- List of notes | end -->
            <div class="add-button">
                <button class="btn" @click="changeStatus('create')">Добавить заметку</button>
            </div>
        </div>
        <!-- EditNote (component) -->
        <div v-if="adding === 'edit'">
            <EditNote :note="selectedNote" @editNote="onEditNote" @confirm="deleteNote" @cancel="onCancelEdit"/>
        </div>
        <!-- CreateNote (component) -->
        <div v-if="adding === 'create'">
            <CreateNote @newNote='onNewNote'/>
        </div>
    </div>
</template>
<!-- Main page | end -->

<script>

    import Note from '../Note.vue'
    import CreateNote from './CreateNote.vue'
    import Modal from '../Modal'
    import EditNote from './EditNote'

    export default {
        name: "Home",

        data() {
            return {
                adding: 'default',
                alert: false,
                delete_status: false,
                notes: [],
                selectedNote: null,
                note: null,
                alert_type: ''
            }
        },
        mounted() {


            if (localStorage.getItem('notes')) {
                try {
                    this.notes = JSON.parse(localStorage.getItem('notes'));
                } catch (e) {
                    localStorage.removeItem('notes');
                }
            }
        },
        methods: {
            changeStatus: function (newState) {
                this.adding = newState;
                console.log(this.adding)
            },
            confirmEdit: function (newState, note) {
                this.selectedNote = note;
                this.adding = newState;
                console.log(this.adding)
            },
            onNewNote(data) {
                this.notes.push(data.note);
                this.adding = data.adding;
                this.saveNotes();
            },
            openModel(note, alert_type) {
                this.selectedNote = note;

                if (alert_type === 'cancel') {
                    this.alert_type = 'cancel'
                }
                if (alert_type === 'delete') {
                    this.alert_type = 'delete'
                }
                this.alert = true;

            },
            cancelDelete() {
                this.alert = false;
                this.selectedNote = null;
            },
            deleteNote(data) {
                this.alert = false;
                let index = this.notes.indexOf(data ? data.note : this.selectedNote);
                this.notes.splice(index, 1);
                this.selectedNote = null;
                this.saveNotes();
                if (data) {
                    this.adding = data.adding;
                }
            },
            saveNotes() {
                const parsed = JSON.stringify(this.notes);
                console.log(localStorage.setItem('notes', parsed));
            },
            onEditNote(data) {
                let index = this.notes.indexOf(data.note);

                if (~index) {
                    this.notes[index] = data.note;
                }

                this.saveNotes();

                this.adding = data.adding;
            },
            onCancelEdit() {
                this.adding = 'default';
            }
        },
        components: {
            Note, CreateNote, Modal, EditNote
        },
    }
</script>

