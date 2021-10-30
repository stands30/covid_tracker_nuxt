<template>
   <div class="card">
       <div class="card_title" v-html="title"></div>
       <div class="card_new_count" v-html="new_count"></div>
       <div class="card_desc" v-html="desc"></div>
   </div>
</template>

<script>
export default {
 props:['title', 'total_count','desc', 'new_count']
}
</script>

<style>
.card{
    width: 20%;
    padding: 20px 40px 50px;
}
</style>