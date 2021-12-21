<template>
    <div class="container">
        <CreatePost @createPost="getPosts"/>
        <h2>Посты</h2>
        <div class="row">
            <div
                v-for="post in posts"
                :key="post.id"
                class="posts col-6 mb-3"
            >
                <div class="posts__card card w-100">
                    <div class="posts__body card-body">
                        <h5 class="posts__title card-title font-weight-bold">
                            {{ post.title }}
                        </h5>
                        <p class="posts__description card-text text-truncate">
                            {{ post.description }}
                        </p>
                        <router-link
                            class="posts__link"
                            :to="{ name: 'Post', params: { id: post.id }}">
                            Перейти
                        </router-link>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import CreatePost from "../../components/CreatePost";

export default {
    name: "Posts",
    components: {
        CreatePost,
    },
    data() {
        return {
            posts: [],
        }
    },
    methods: {
        async getPosts() {
            this.posts = (await axios.post('/api/posts')).data
        }
    },
    mounted() {
        this.getPosts()
    }
}
</script>

<style lang="scss" scoped>

</style>
