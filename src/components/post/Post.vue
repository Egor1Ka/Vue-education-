<template>
    <div class="post" key="id">
        <div><strong>{{id}}</strong> {{title}}</div>
        <div>{{body}}</div>
        <button @click="$emit('remove', id)">delete</button>
    </div>
</template>

<script>
export default {
    props:{
        title:{
            type: String,
        },
        body:{
            type: String
        },
        id:{
            type: String
        }
    }
}
</script>


<style scoped>
.post{
    padding: 5px;
    border:1px solid black;
    margin: 10px 0px;
}
</style>