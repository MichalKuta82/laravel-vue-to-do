<template>
	<div class="app-component table-responsive">

		<table class="table">
		  <thead class="thead-light">
		    <tr>
		      <th>Id</th>
		      <th scope="col">Task Title</th>
		      <th scope="col">Priority</th>
		      <th scope="col">Action</th>
		    </tr>
		  </thead>
		  <tbody>
		  	<task-component v-for="task in tasks" :key="task.id" :task="task"></task-component>
		  	<tr>
			    <td><input v-model="task.title" type="text" name="" id="task" class="form-control"></td>
			    <td>
			    	<select v-model="task.priority" id="select" class="form-control">
			    		<option>Low</option>
			    		<option>Medium</option>
			    		<option>High</option>
			    	</select>
			    </td>
			    <td><button @click="store" class="btn btn-primary">Add Task</button></td>
			</tr>
		  </tbody>
		</table>
		
	</div>

</template>

<script>
	import TaskComponent from './Task.vue';

	export default {

		data(){
			return{
				tasks: [],
				task:{
					title: '',
					priority: ''
				},
				message: 'Hello from kupa'
			}
		},

		methods: {
			getTasks(){
				window.axios.get('api/tasks/').then(({data}) => {
					data.forEach(task => {
						this.tasks.push(task)
					});
				});
			},
			store(){
				window.axios.post('/api/tasks', this.task).then(savedTask => {
					this.tasks.push(savedTask.data);
				});
			}
		},

		created(){
			this.getTasks();
		},

		components:{TaskComponent}
	}

</script>

<style>
	
	
</style>