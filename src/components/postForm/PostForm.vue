<template>
    <div>
        <form @submit.prevent>
            <input v-model="post.title" class="title input" placeholder="title">
            <input v-model="post.body" class="body input" placeholder="body">
            <button @click="createPost">add post</button>
        </form>
    </div>
</template>

<script>
export default {
    data: () => ({
        post:{
            title:"",
            body:""
        }
       
    }),
    methods:{
        createPost(){
            const id = Date.now();
            this.post.id = id;
            
            this.$emit("create",this.post)

            this.post = {
                title:"",
                body:""
            }
        }


    }
}
</script>

<style scoped>
.input{
    width: 100%;
    padding: 10px;
    margin: 10px 0px;
}
</style>
