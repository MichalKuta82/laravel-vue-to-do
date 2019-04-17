<template>
	<div class="app-component table-responsive">
		<loading :active.sync="isLoading"></loading>

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
		  	<task-component v-for="task in tasks" :key="task.id" :task="task" @delete="remove"></task-component>
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
	// Import component
    import Loading from 'vue-loading-overlay';
    // Import stylesheet
    import 'vue-loading-overlay/dist/vue-loading.css';
	import TaskComponent from './Task.vue';

	export default {

		data(){
			return{
				isLoading: false,
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
				window.axios.get('/api/tasks/').then(({data}) => {
					data.forEach(task => {
						this.tasks.push(task)
					});
				});
			},
			store(){
				if (this.checkInputs()) {
					this.isLoading = true;
					window.axios.post('/api/tasks', this.task).then(savedTask => {
					this.tasks.push(savedTask.data);	
					this.task.title = '';
					this.isLoading = false;
					});
				}
			},
			checkInputs(){
				if (this.task.title && this.task.priority) {
					return true;
				}
			},
			remove(id){
				this.isLoading = true;
				window.axios.delete(`/api/tasks/${id}`).then(() => {
					let index = this.tasks.findIndex(task => task.id === id);
					this.tasks.splice(index, 1);
					this.isLoading = false;
				});
			}
		},

		created(){
			this.getTasks();
		},

		components:{TaskComponent, Loading}
	}

</script>

<style>
	
	
</style>