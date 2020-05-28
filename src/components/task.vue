<template>
    <div class="task" :class="stateClass" @click="changeState()">
        <span class="close" @click="deleteTask()">X</span>
        <p>{{task.name}}</p>
    </div>
</template>

<script>

import eventBus from '@/components/eventBus'

export default {
    props: {
        task: {type: Object , required: true}
    },
    computed:{
        stateClass(){
            return {
                pending: this.task.pending,
                done: !this.task.pending
            }
        }
    },
    methods:{
        changeState(){
            this.task.pending = !this.task.pending
        },
        deleteTask(){
            eventBus.$emit('taskDeleted', this.task)
        }
    }
}
</script>

<style>
    .task{
        position: relative;
        box-sizing: border-box;
        width: 350px;
        height: 150px;
        padding: 10px;
        border-radius: 10px;
        font-size:  20px;
        font-weight: 100;
        cursor: pointer;
        user-select: none;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .pending{
        border-left: 12px solid #ec6400;
        background-color: #ff8229;
    }

    .done{
        color: white;
        border-left: 12px solid #007c42;
        background-color: #00bd52;
        text-decoration: line-through;
    }
    
    .task .close{
        background-color: #ec6400;
    }

    .done .close{
        background-color: #007c42;
    }

    .close{
        position: absolute;
        right: 10px;
        top: 10px;
        font-weight:bold;
        font-size: 15px;
        height: 20px;
        width: 20px;
        border-radius: 100%;
        display: flex;
        justify-content: center;
    }

</style>