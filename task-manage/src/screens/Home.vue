<template>
<div>
	<AddTask v-show="showAddTask" @add-task="addTask" />
	<Tasks 
		@toggle-reminder="toggleReminder" 
		@delete-task="deleteTask" :tasks="tasks" 
	/>
</div>
</template>

<script>
import Tasks from '../components/Tasks.vue'
import AddTask from '../components/AddTask.vue'
import DropDown from '../components/DropDown.vue'

export default {
	name: 'Home',
	props: {
		showAddTask: Boolean,
	},
	components: {
	Tasks,
	AddTask,
	DropDown
  },
  data() {
	return {
		tasks: [],
		check: '',
		jobTitles: [
			{ name: "Product designer", id: 1 },
     		{ name: "Full-stack developer", id: 2 },
    		{ name: "Product manager", id: 3 },
      		{ name: "Senior front-end developer", id: 4 }
    	],
    	selectedJobTitle: null
	}
  },
   methods: {
	changeJobTitle (event) {
      this.selectedJobTitle = event.target.options[event.target.options.selectedIndex].text
    },
	async addTask(task) {
		const res = await fetch('api/tasks', {
			method: 'POST',
			headers: {
				'Content-type': 'application/json',
			},
			body: JSON.stringify(task)
		})

		const data = await res.json()
		this.tasks = [...this.tasks, data]
	},
	async deleteTask(id) {
		if (confirm('Are you sure?')) {
		const res = await fetch(`api/tasks/${id}`, {
			method: 'DELETE',
		})
		res.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert("error deleting task")
		}
	},
	async toggleReminder(id) {
		const taskToToggle = await this.fetchTask(id)
		const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

		const res = await fetch(`api/tasks/${id}`,{
			method: 'PUT',
			headers: {
				'Content-type': 'application/json',
			},
			body: JSON.stringify(updTask)
		})

		const data = await res.json()
		this.tasks = this.tasks.map((task) => 
		task.id === id ? {...task, reminder: data.reminder} : task
		)
	},
	async fetchData() {
		const res = await fetch('api/tasks')

		const data = await res.json()

		return data
	},
	async fetchTask(id) {
		const res = await fetch(`api/tasks/${id}`)

		const data = await res.json()

		return data
	},
  },
  async created() {
	this.tasks = await this.fetchData()
	console.log(localStorage.getItem('reminder'))
  }
	
}
</script>