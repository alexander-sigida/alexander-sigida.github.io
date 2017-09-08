<template>
	<div class="container">
		<div class="row mgt40">
			<div class="col text-center">
				<h1>My awesome everyday To Do List</h1>
			</div>
		</div>
		<div class="row mgt40">
			<div class="col-12">
				<form>
					<p class='mgb10'>Add your tasks for today below</p>
					<input class='mgb10 form-control' type="text" v-model="task">
					<button class="btn btn-primary" @click.prevent="addNewTask(task)">Add</button>
				</form>
			</div>
		</div>



		<!-- separate component -->
		<div class="row mgt40">

			<div class="col-12">
				<h2 class='text-center'>Your tasks for today</h2>
			</div>
			<transition enter-active-class="animated fadeIn fast" leave-active-class="animated fadeOut fast" mode="out-in">
				<div key='activeTasks' v-if="anyActiveTasks" class="col-12">	
					<div class='tasks-wrapper'>
						<transition-group enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
							<div key='index' v-for="(task, index) in activeTasks" class='mgb10 task-wrapper'>
								<div  class="input-group">
									<span class="input-group-btn">
										<button class="btn btn-lg btn-success" @click.prevent="completeTask(index)"  type="button">Done</button>
									</span>
									<input type="text" class="form-control task" :value="task" aria-label="Search for...">
									<span class="input-group-btn">
										<button class="btn btn-lg btn-danger" @click.prevent="deleteTask(index)" type="button">Delete</button>
									</span>
								</div>
							</div>
						</transition-group>
					</div>
				</div>

				<div key='noActiveTasks' v-else class='col-12 text-center mgt40'>
					<div class='alert alert-success'>
						You have no active tasks for today :)
					</div>
				</div>
			</transition>	

		</div>
		<!-- /// -->



		<div class="row mgt40">

			<div class="col-12">
				<h2 class='text-center'>Completed tasks</h2>
			</div>

			<transition enter-active-class="animated fadeIn fast" leave-active-class="animated fadeOut fast" mode="out-in">
				<div key='completedTasks' v-if="anyCompletedTasks" class='col-12'>
					<div class="tasks-wrapper">
						<transition-group enter-active-class="animated fadeIn" leave-active-class="animated fadeOut" mode="out-in">
							<div key="index" v-for="(completedTask, index) in completedTasks" class="mgb10">
								<div class="alert alert-success" role="alert">
									{{ completedTask }} 
								</div>
							</div>
						</transition-group>
					</div>
					<p><button id='clearTasksButton' @click="clearCompletedTasks" class="btn btn-primary">Clear</button></p>
				</div>

				<div key='noCompletedTasks' v-else class="col-12 mgt40 text-center">
					<div class="alert alert-secondary" role="alert">
						You have no completed tasks today
					</div>
				</div>
			</transition>			

		</div>
	</div>
</template>

<script>
	export default ({
		data: function() {
			return {
				activeTasks: [],
				task: '',
				completedTasks: [
				],
				typeOfTasksToUpdate: {
					databaseArrayName: '',
					jsArray: []
				},
				empty: []

			}
		},
		methods: {
			updateDataBase(dataBaseNode, jsArray) {
				this.$http.put('https://vuejs-http-alexander.firebaseio.com/' + dataBaseNode + '.json', jsArray).then(response => {
					console.log(response);	
				}, error => {
					console.log(error);
				}); 
			},
			fetchData(dataBaseNode, jsArray) {
				this.$http.get('https://vuejs-http-alexander.firebaseio.com/' + dataBaseNode + '.json').then(response => {
					return response.json();	
				})
					.then(returnedData => {
						for (let counter in returnedData) {
							jsArray.push(returnedData[counter]);
						}
					})
			},
			addNewTask(task) {
				this.activeTasks.push(task);
				/*
				this.typeOfTasksToUpdate.databaseArrayName = 'activeTasks';
				this.typeOfTasksToUpdate.jsArray = this.activeTasks;
				*/
				this.updateDataBase('activeTasks', this.activeTasks);
				this.task = "";
			},
			deleteTask(index) {
				this.activeTasks.splice(index, 1);
				this.updateDataBase('activeTasks', this.activeTasks);
				this.updateDataBase();
			},
			completeTask(index) {
				const completedTask = this.activeTasks.splice(index, 1);
				this.completedTasks.push(completedTask[0]);
				// delete from activeTasks on the database
				this.updateDataBase('activeTasks', this.activeTasks);
				this.updateDataBase();
				// update completedTasks
				this.updateDataBase('completedTasks', this.completedTasks);
				this.updateDataBase();
			},
			clearCompletedTasks() {
				this.completedTasks.splice(0, this.completedTasks.length); 
				this.updateDataBase('completedTasks', this.empty);
			}
		},
		computed: {
			anyActiveTasks: function() {
				if ( this.activeTasks.length == 0 ) {
					return false
				} else {
					return true
				}	
			},
			anyCompletedTasks: function() {
				if ( this.completedTasks.length == 0 ) {
					return false
				} else {
					return true
				}	
			}
		},
		created() {
			this.fetchData('activeTasks', this.activeTasks);
			this.fetchData('completedTasks', this.completedTasks);
		}
		 
	})
</script>

<style scoped>
	.mgt40 {
		margin-top: 40px;
	}
	.mgb40 {
		margin-bottom: 40px;
	}
	.mgb10 {
		margin-bottom: 10px;
	}
	.tasks-wrapper {
		margin-top: 40px;
	}
	.task-wrapper:last-child {
		margin-bottom: 0;
	}
	.task {
		line-height: 30px;
		font-size: 20px;
		font-weight: 500;
		padding-right: 32px;
	}
	.icon-task-delete {
		font-size: 40px;
		top: -15px;
		right: 12px;
		position: absolute;
		cursor: pointer;
		z-index: 4;
	}
	button {
		cursor: pointer;
	}
	.fadeIn {
		transition: .2s;
	}
	.fadeOut {
		transition: .2s;
	}
	.fast {
		transition: .1s;
	}
	#clearTasksButton {
		float: right;
	}
</style>
