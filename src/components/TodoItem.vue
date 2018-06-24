<template>
    <li>
        <div>
            <input type="checkbox" v-model="item.completed"/>
            <label :class="{completed:item.completed}">{{ item.title }}</label>
            <button @click="removeItem(index)">删除</button>
        </div>       
    </li>
</template>

<script>
export default {
    props:['item','index'],
    data:()=>{
        return{
            message:"",
        }
    },
    methods:{
        removeItem: function(index){
            this.$emit("removeItem",index)
        }
    }    
}
</script>

<style scoped>
li{
    list-style:none;
}
.completed{
    text-decoration : line-through;
}

</style>


