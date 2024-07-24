<template>
    <div>
        <h1>BUNA!!!
            Blogul GGIT FD</h1>

            <img href="https://i3.cpcache.com/merchandise_zoom/115741933/124_750x750_Front_Color-NA.jpg?region={%22name%22:%22FrontCenter%22,%22width%22:2.6950095,%22height%22:2.7,%22alignment%22:%22MiddleCenter%22,%22orientation%22:0,%22dpi%22:300,%22crop_x%22:0,%22crop_y%22:0,%22crop_h%22:540,%22crop_w%22:539,%22scale%22:0.99815154,%22template%22:{%22id%22:115741933,%22params%22:{}}}&AttributeValue=NA&c=False&cid=PUartJBjiF%2Fyg4FdKqiggQ%3D%3D+%7C%7C+yPvjkVsGsggUtyS84ab0YQ%3D%3D&ProductNo=436845707">
            <img href="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQYIFwO6XDBdicf6P_BD8WicDN8plv08OrL2g&s">

        <div v-if="blogList.length>0">
            <BlogItem v-for="blog in blogList"
            :key="blog.id"
            :id="blog.id"
            :user="blog.user"
            :post="blog.post"
            :likes="blog.likes"
            :tag="blog.tag"
            @like="likeHandler($event)"
            @dislike="dislikeHandler($event)"
            />
        </div>
        <div v-else>
            <p>Nici o postare existenta</p>
        </div>

        <div>
            <input v-model="user"/>
            <br>
            <textarea v-model="post"></textarea>
            <br>
            <select v-model="tag">
                <option value="general">General</option>
                <option value="flame">Flame</option>
                <option value="recommendation">Recommendation</option>
            </select>

            <button @click="submitFormHandler">Save</button>
        </div>
    </div>
</template>

<script>
import BlogItem from '../components/BlogItem.vue'
import {myaxios} from '../axios'
    export default {
        components: {BlogItem},
        data() {
            return {
                user: "",
                post: "",
                tag: "general",
                blogList: []
            }
        },
        methods: {
            submitFormHandler(event) {
                const newBlog = {
                    user: this.user,
                    post: this.post,
                    tag: this.tag
                }
                myaxios.post("/blog", newBlog).then((response) =>{
                    this.blogList = response.data.blogs
                })

                console.log(newBlog)
                },

                fetchBlogPosts(){
                    const blogs = myaxios.get("/blog").then(
                        (response) => {
                            console.log(response.data.blogs)
                            this.blogList = response.data.blogs
                        }
                    )
                },
                
                    likeHandler(event){
                        console.log(event.itemId)
                        myaxios.post(`/blog/${event.itemId}/like`).then((response) => {
                            console.log(response.data.blogs)
                            this.blogList = response.data.blogs
                        })
                    },
                    dislikeHandler(event){
                        console.log(event.itemId)
                        myaxios.post(`/blog/${event.itemId}/dislike`).then((response) => {
                            console.log(response.data.blogs)
                            this.blogList = response.data.blogs
                        })
                    }
            },
            mounted () {
                this.fetchBlogPosts()
            },
        }
</script>

<style scoped>
h1 {
color:rgb(69, 30, 14)
}
body {
background-color:burlywood
}
</style>