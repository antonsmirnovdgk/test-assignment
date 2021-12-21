<template>
    <form class="border p-3 rounded mb-3 bg-dark shadow">
        <div class="form-group">
            <input
                v-model="form.title"
                class="form-control mb-1"
                type="text"
                id="title"
                placeholder="Имя">
        </div>
        <div class="form-group ">
            <textarea
                v-model="form.description"
                placeholder="Комментарий"
                class="form-control mb-1"
                id="comment"
                rows="3">
            </textarea>
        </div>
        <div class="d-flex">
            <button
                class="btn btn-primary mb-3 mr-3"
                @click="editPost"
            >
                Редактировать
            </button>
            <button
                class="btn btn-primary mb-3 mr-3"
                @click="cancelEdit"
            >
                Отменить
            </button>
        </div>

    </form>
</template>

<script>
export default {
    name: "EditPost",
    props: ['value'],
    data() {
        return {
            form: {
                title: this.value.title,
                description: this.value.description,
            },
        }
    },
    methods: {
        editPost() {
            this.$emit('editPost', this.form)
        },
        cancelEdit() {
            this.$emit('cancelEdit')
        }
    }
}
</script>

<style scoped>

</style>
