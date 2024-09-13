<template>
  <div class="dialog" v-if="show" @click.stop="$emit('update:show',false)">
    <div @click.stop class="dialog__content">
        <slot/>
    </div>
  </div>
</template>

<script>
export default {
    nam:'my-dialog',
    props:{
        show: {
            type: Boolean,
            default: false
        }
    }
}
</script>

<style>
.dialog{
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.5);
    position: fixed;
    display: flex;
}

.dialog__content{
    margin: auto;
    background: white;
    border-radius: 12px;
    min-height: 50px;
    min-width: 300px;
    padding: 20px;
}
</style>