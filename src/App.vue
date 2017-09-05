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
							<div key='index' v-for="(task, index) in tasks" class='mgb10 task-wrapper'>
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
				tasks: [
					'Fill the ToDo List',
				],
				task: '',
				completedTasks: [
				]
			}
		},
		methods: {
			addNewTask(task) {
				this.tasks.push(task);
				this.task = "";
			},
			deleteTask(index) {
				this.tasks.splice(index, 1);
			},
			completeTask(index) {
				const completedTask = this.tasks.splice(index, 1);
				this.completedTasks.push(completedTask[0]);
			}
		},
		computed: {
			anyActiveTasks: function() {
				if ( this.tasks.length == 0 ) {
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
</style>
