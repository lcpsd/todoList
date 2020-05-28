<template>
	<div id="app">
		<h1>Tarefas</h1>
		<taskProgress :progress="progress" />
		<newTask @taskAdded="addTask"/>
		<taskGrid :tasks="tasks"/>
	</div>
</template>

<script>
import taskProgress from './components/taskProgress'
import newTask from './components/newTask.vue'
import taskGrid from './components/taskGrid.vue'
import eventBus from './components/eventBus'

export default {
	components: {taskGrid, newTask,taskProgress},
	data(){
		return{
			tasks: new Array
		}
	},
	computed:{
		progress(){
			const total = this.tasks.length
			const done = this.tasks.filter(t => !t.pending).length
			return Math.round(done / total * 100) || 0
		}
		
	},
	methods:{
		addTask(task){
			const sameName = (t) => t.name === task.name
			const reallyNew = this.tasks.filter(sameName).length == 0

			reallyNew && this.tasks.push({
				name: task.name,
				pending: task.pending || true
			})
		}
	},

	created(){
		eventBus.$on('taskDeleted', (task) => {
			
			const checkName = (element) => element == task.name
			const taskIndex = this.tasks.findIndex(checkName)

			taskIndex ? this.tasks.splice(taskIndex, 1) : null
		})
	}
}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background-color: #131e2e;
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
