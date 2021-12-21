<template>
    <form
        class="form-comment border p-3 rounded mb-3 shadow"
        @submit.stop.prevent="createComment">
        <div class="form-comment__group form-group" :class="{hasError: isError}">
            <label for="title">Имя</label>
            <input
                v-model="form.name"
                class="form-comment__input-name input form-control mb-1"
                type="text"
                id="title"
                placeholder="Имя">
            <span class="error" :class="{hasError: isError}">{{ this.errors.nameError }}</span>
        </div>
        <div class="form-comment__group form-group" :class="{hasError: isError}">
            <label for="comment">Комментарий</label>
            <textarea
                v-model="form.text"
                placeholder="Комментарий"
                class="form-comment__input-textarea textarea form-control mb-1"
                id="comment"
                rows="3">
            </textarea>
            <span class="error" :class="{hasError: isError}">{{ this.errors.textError }}</span>
        </div>
        <button type="submit" class="form-comment__btn btn btn-primary mb-3">Прокомментировать</button>
    </form>
</template>

<script>


export default {
    name: "CreateComment",
    props: ['id'],

    data() {
        return {
            isError: false,
            form: {
                name: '',
                text: '',
            },
            errors: {
                nameError: '',
                textError: '',
            }
        }
    },
    methods: {
        async createComment() {
            try {
                await axios.post('/api/comments/create', {...this.form, post_id: this.id});

                this.form.title = '';
                this.form.description = '';
                this.errors.titleError = '';
                this.errors.descriptionError = '';

                this.$emit('createComment');
            } catch (err) {
                this.isError = true;

                this.errors.nameError = err.response.data.errors.name[0];
                this.errors.textError = err.response.data.errors.text[0]
            }
        },
    },

    watch: {
        form : {
            deep: true,
            immediate: true,

            handler(){
                if(this.form.name !== "" || this.form.text !== ""){
                    this.isError = false
                }
            }
        }
    }
}
</script>

<style lang="scss" scoped>
.input, .textarea {
    .hasError & {
        border-color: red;
        transition: 1s;
    }
}

.hasError label {
    color: red;
    transition: 1s;
}

.error {
    display: inline-block;

    transform: translateY(-15px);
    opacity: 0;
    transition: 1s;
}

.hasError {
    color: red;
    border-color: red;

    transform: translateY(0);
    transition: 1s;
    opacity: 1;
}
</style>
