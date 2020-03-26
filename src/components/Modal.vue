<template>
    <div class="alert">
        <div class="alert__delete">
            <p>Вы уверены что хотите {{type === 'delete' ? 'удалить заметку' : 'отмень редактирование редактировать'}} <br/><b>{{ note.title }}?</b></p>
            <button class="btn btn_green" @click="onConfirm" v-if="type === 'delete'">Да</button>
            <button class="btn btn_green" @click="onConfirmCancel" v-if="type === 'cancel'">Да</button>
            <button class="btn btn_red" @click="onCancel">Нет</button>
        </div>
    </div>
</template>

<script>
    export default {
        name: "DeleteAlert",

        props: ["open", "note", "type"],

        methods: {
            onConfirm() {
                this.$emit("confirm");
            },
            onConfirmCancel() {
                this.$emit("confirmCancel");
            },
            onCancel() {
                this.$emit("cancel");
            }
        }
    }
</script>
