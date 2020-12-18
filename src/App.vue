<template>
	<div id="app">
		<h1>Tarefas</h1>
		<task-progress :progress="progress"/>
		<NewTask @taskAdded="addTask($event)"/>
		<TaksGrid
			@taskDeleted="deleteTask"
			@taskStateChanged="toggleTaskState"
			:tasks="tasks"
		/>
	</div>
</template>

<script>
import NewTask from './components/NewTask.vue'
import TaksGrid from './components/TaskGrid.vue'
import TaskProgress from './components/TaskProgress.vue'

export default {
	components: {TaksGrid, NewTask, TaskProgress},
	data(){
		return{
			tasks:[
				// {name: 'Assisir video aulas', pending: false},
				// {name: 'Fazer exercícios', pending: true},
			]
		}
	},
	computed:{
		progress(){
			const total = this.tasks.length
			const done = this.tasks.filter(t => !t.pending).length
			return 	Math.round(done/total *100) || 0
		}
	},
	watch:{
		tasks:{
			deep:true,
			handler(){
				localStorage.setItem('tasks', JSON.stringify(this.tasks))
			}
		}
	},
	methods:{
		addTask(task){
			const sameName = t => t.name === task.name
			const reallyNew = this.tasks.filter(sameName).length == 0

			// verifica se a tarefa é nem nome repetido
			if(reallyNew){
				this.tasks.push({
					name: task.name,
					pending: task.pending || true
				})
			}else{
				alert("Não é permitido adicionar uma tarefa com o nome repetido")
			}
		},
		deleteTask(task){
			//busca index do tarefa
			const i = this.tasks.indexOf(task)
			
			// deleta tarefa da lista pelo index
			this.tasks.splice(i,1)
		},
		toggleTaskState(task){
			//busca index do tarefa
			const i = this.tasks.indexOf(task)			

			this.tasks[i].pending = !this.tasks[i].pending
		}
	},
	// poupula a lista tasks com dados do LocalStorage
	created(){
		const json = localStorage.getItem('tasks')
		const array = JSON.parse(json)
		this.tasks = Array.isArray(array) ? array : []
	}

}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
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
