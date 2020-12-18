<template>
	<div id="app">
		<h1>Tarefas</h1>
		<NewTask @taskAdded="addTask($event)"/>
		<TaksGrid :tasks="tasks"/>
	</div>
</template>

<script>
import NewTask from './components/NewTask.vue'
import TaksGrid from './components/TaskGrid.vue'

export default {
	components: {TaksGrid, NewTask},
	data(){
		return{
			tasks:[
				{name: 'Assisir video aulas', pending: false},
				{name: 'Fazer exercícios', pending: true},
			]
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
		}
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
