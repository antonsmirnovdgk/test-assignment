<template>
    <div class="mb-3 shadow">
        <div class="column align-items-center">
            <div class="card">
                <div class="card-header font-weight-bold bg-secondary text-light">Создать пост</div>
                <div class="card-body">
                    <form class="createForm" @submit.stop.prevent="createPost">
                        <div class="form-group" :class="{hasError: isError}">
                            <label class="text-bold"
                                   for="title"
                            >
                                Название поста
                            </label>
                            <input
                                v-model="form.title"
                                class="input form-control mb-1"
                                type="text"
                                id="title"
                                placeholder="Название">
                            <span class="error" :class="{hasError: isError}">{{ this.errors.titleError }}</span>
                        </div>
                        <div class="form-group" :class="{hasError: isError}">
                            <label for="textArea">Описание</label>
                            <textarea
                                v-model="form.description"
                                placeholder="Описание"
                                class="textarea form-control mb-1"
                                id="textArea"
                                rows="3">
                            </textarea>
                            <span class="error" :class="{hasError: isError}">{{ this.errors.descriptionError }}</span>
                        </div>
                        <button type="submit" class="btn btn-light ">Создать</button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>


export default {
    name: "CreatePost",
    data() {
        return {
            isError: false,
            form: {
                title: '',
                description: '',
            },
            errors: {
                titleError: '',
                descriptionError: '',
            }
        }
    },
    methods: {
        async createPost() {
            try {
                await axios.post('/api/posts/create', this.form);

                this.form.title = '';
                this.form.description = '';
                this.errors.titleError = '';
                this.errors.descriptionError = '';

                this.$emit('createPost');
            } catch (err) {
                this.isError = true;

                this.errors.titleError = err.response.data.errors.title[0];
                this.errors.descriptionError = err.response.data.errors.description[0]
            }
        },

    },

    watch: {
        form : {
            deep: true,
            immediate: true,

            handler(){
                if(this.form.title !== "" || this.form.description !== ""){
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
