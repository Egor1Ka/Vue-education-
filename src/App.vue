<template>
<div>
    <button @click="openWindow">OpenWindow</button>
    <my-select
        v-model="selectedSort"
        :options="[{name: 'По названию', value: 'title'}, {name: 'По описанию', value: 'body'}]"
    />
    <input v-model="searchQuery"/>
    <div v-if="isPostLoading">LOADING POST</div>
    <post-list :posts ="posts" @remove="deletePost"/>
    <div ref="observer" class="observer"/>
    <my-dialog v-model:show="show"> 
         <post-form @create="createPost"/> 
    </my-dialog>
    <div v-for="pageNumber in totalPages" :key="pageNumber">
        <button :class="{'active': pageNumber === page}">{{pageNumber}}</button>
    </div>
</div>
</template>

<script>
import PostList from './components/postList/PostList.vue'
import PostForm from './components/postForm/PostForm.vue'
import MyDialog from './components/UI/MyDialog.vue'
import axios from 'axios'
import MySelect from './components/UI/MySelect.vue'

export default {
  components: { PostList, PostForm, MyDialog, MySelect },
  name: 'App',
  data: () => ({
    posts: [],
    show: false,
    isPostLoading: true,
    selectedSort: '',
    searchQuery:'',
    page:1,
    limit:10,
    totalPages:10
  }),
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.show = false;
    },
    deletePost(id) {
      console.log('delete', id);
      this.posts = this.posts.filter(p => p.id !== id);
    },
    openWindow() {
      this.show = true;
    },
   /* changaPageHandler(page){
        this.page = page;
    },*/
    loadMorePosts(){
        this.fetchPosts().then(posts => {
            this.posts = [...this.posts, ...posts];
            this.isPostLoading = false;
        })
    },
    async fetchPosts() {
      try {
        const response = await axios.get(`https://jsonplaceholder.typicode.com/posts`,{
            params:{
                _limit: this.limit,
                _page: this.page
            }
        });
        return response.data;
      } catch (e) {
        console.log('e', e);
      }
    }
  },
  mounted() {
    this.fetchPosts().then(posts => {
      this.posts = posts;
      this.isPostLoading = false;
    });

    const options = {
        rootMargin: "0px",
        threshold: 1.0,
    };

    const observer = new IntersectionObserver(()=>{
        this.page = this.page + 1;
        this.loadMorePosts();
    }, options);
    observer.observe(this.$refs.observer);
  },
  
  watch: {
    selectedSort(newValue) {
      console.log('newValue', newValue);
      this.posts.sort((post1, post2) => post1[newValue].localeCompare(post2[newValue]));
    },
    posts(newPost){
        console.log('newPost',newPost);
    },
    /*page(){
        this.fetchPosts().then(posts => {
            this.posts = posts;
            this.isPostLoading = false;
        })
    }*/
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.post {
  padding: 5px;
  border: 1px solid black;
  margin: 10px 0px;
}

.active{
    border:15px solid red;
}

.observer{
    height: 30px;
    background: green;
}
</style>
