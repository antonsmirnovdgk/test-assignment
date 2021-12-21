<template>
    <div>
        <div class="post p-3 mb-5 border rounded shadow">
            <!--            <p class="lead">-->
            <!--                Номер поста {{id}}-->
            <!--            </p>-->
            <div v-if="editPost" class="post__wrapper mb-5">
                <h1 class="post__title mb-5 text-center text-uppercase">
                    {{ post.title }}
                </h1>
                <p class="post__description border-bottom">{{ post.description }}</p>
            </div>
            <EditPost
                v-else
                @editPost="remadePost"
                @cancelEdit="toggleEdit"
                v-model="post"/>
            <div class="post__buttons">
                <button v-if="editPost" @click="toggleEdit" class="btn text-lowercase font-weight-bold">Редактировать пост</button>
                <button v-if="editPost" @click="deletePost" class="btn text-lowercase font-weight-bold">Удалить пост</button>
            </div>
        </div>
        <CreateComment
            :id="this.id"
            @createComment="getPost"
        />
        <div class="comments">
            <p class="font-weight-bold">Комментарии</p>
            <div
                v-for="item in post.comments"
                :key="item.id"
                class="card mb-2"
            >
                <div class="card-body bg-dark rounded shadow">
                    <h5 class="card-title text-warning">
                        Имя: {{ item.name }}
                    </h5>
                    <p class="card-text text-light">
                        {{ item.text }}
                    </p>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
import CreateComment from "../../components/CreateComment";
import EditPost from "../../components/EditPost";

export default {
    name: "Post",
    props: ['id'],
    components: {
        CreateComment,
        EditPost
    },
    data() {
        return {
            post: [],
            editPost: true,
        }
    },
    methods: {
        async getPost() {
            this.post = (await axios.post('/api/post', {id: this.id})).data
        },
        toggleEdit() {
            this.editPost = !this.editPost
        },
        remadePost(data) {
            axios.post('/api/posts/update', {id: this.id,...data })
            this.getPost()
            this.toggleEdit()
        },
        async deletePost(data) {
            try {
                await axios.post('/api/posts/delete', {id: this.id,...data })
                await this.$router.push({path: '/posts'})
            }
             catch (err) {
                console.log(err)
             }
        }
    },

    mounted() {
        this.getPost()
    }
}
</script>

<style scoped>

</style>
