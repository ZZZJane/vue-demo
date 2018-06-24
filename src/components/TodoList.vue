<template>
    <div>
        <input v-model = "message" placeholder="输入待办事项" @keyup.enter="addItem" v-focus="isFocus">
        <button @click="addItem">添加</button>
        <ul v-if="list.length">
            <p>共{{ unfinishedLength }}项工作待完成</p><hr>
            <input type='radio' v-model='mode' value='all'/><p>全部任务</p>
            <input type='radio' v-model='mode' value='unfinished'/><p>未完成</p>
            <input type='radio' v-model='mode' value='finished'/><p>已完成</p>
            
            <todo-item    
                v-for="(item,index) in filterItems" 
                :key="index"
                :index="index"
                :item="item"
                @removeItem="removeItem">
            </todo-item>  
        </ul>
        <p v-else style="display:block;">没有待办事项</p>   
    </div>
</template>

<script>
import TodoItem from "./TodoItem.vue";
export default {
    components:{ TodoItem },
    directives: {
        focus: {
            inserted:function(el){
                el.focus();
            },
            update: function (el, {value}) {
                if (value) {
                    el.focus()
                }
            }
        }
    },
    data:()=>{
        return{
            message:"",
            mode:'all',
            isFocus:true,
            list:[
                {
                    title:"数学卷子",
                    completed:false,
                },
            ]
        }
    },
    computed:{
        count () {
            return this.$store.state.count;
        },
        finished:function(){
            return this.list.filter(item=>item.completed);
        },
        unfinished:function(){
            return this.list.filter(item=>!item.completed);
        },
        filterItems:function(){
            switch(this.mode){
                case 'all':
                    return this.list;
                    break;
                case 'finished':
                    return this.finished;
                    break;
                default:
                    return this.unfinished;
            }
        },
        unfinishedLength:function(){
            return this.unfinished.length;
        }
    },
    methods:{
        addItem:function(){
            //this.isFocus = true;
            if(this.message==''){
                return;
            }
            this.list.push({
                title:this.message,
                completed:false
            });
            this.message = '';
        },
        removeItem:function(index){
            this.list.splice(index,1);
        }
    }
    
}
</script>

<style scoped>
p{
    display:inline;
}
.completed{
    text-decoration : line-through;
}

</style>


