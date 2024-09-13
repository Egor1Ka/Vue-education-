<template>
   <div v-if="posts.length > 0">
    <transition-group name='post-list'>
        <post
        v-for = "post in posts" 
        :key="post.id" 
        :title ="post.title" 
        :body="post.body" 
        :id="post.id"
        @remove="$emit('remove', post.id)"
        />
    </transition-group>
        </div>
   
    <h2 v-else>No posts</h2>
   
</template>

<script>
import Post from '../post/Post.vue'
export default {
    components: {
        Post
    },
    props: {
        posts: {
            type: Array,
            required: true
        }
    },
}
</script>

<style scoped>
.post-list-enter-active,
.post-list-leave-active {
  transition: all 0.5s ease;
}
.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}


.post-list-move{
  transition: transform 0.8s ease;
}
</style>


