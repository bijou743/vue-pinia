<template>
	<main>
		<header>
			<img src="./assets/pinia-logo.svg" alt="Pinia logo" />
			<h1>Pinia Tasks</h1>
		</header>

		<!-- task form -->
		<div class="new-task-form">
			<TaskForm />
		</div>

		<!-- filter -->
		<div class="filter">
			<button @click="filter = 'all'">All tasks</button>
			<button @click="filter = 'favs'">Fav tasks</button>
		</div>

		<!-- task list -->
		<transition name="switch" mode="out-in">
			<div class="task-list" v-if="filter === 'all'">
				<p>You have {{ taskStore.totalCount }} tasks left to do</p>
				<transition-group name="list" tag="div">
					<div v-for="task in taskStore.tasks" :key="task.id">
						<TaskDetails :task="task" />
					</div>
				</transition-group>
			</div>

			<div class="task-list" v-else>
				<p>You have {{ taskStore.favCount }} favs left to do</p>
				<div v-for="task in taskStore.favs" :key="task.id">
					<TaskDetails :task="task" />
				</div>
			</div>
		</transition>
	</main>
</template>

<script>
import { ref } from 'vue';
import TaskDetails from './components/TaskDetails.vue';
import { useTaskStore } from './stores/TaskStore';
import TaskForm from './components/TaskForm.vue';

export default {
	components: { TaskDetails, TaskForm },
	setup() {
		const taskStore = useTaskStore();

		const filter = ref('all');

		return { taskStore, filter };
	},
};
</script>

<style>
.list-enter-from {
	opacity: 0;
	transform: scale(0.6);
}

.list-enter-active {
	transition: all 0.4s ease;
}

.list-leave-to {
	opacity: 0;
	transform: scale(0.6);
}

.list-leave-active {
	transition: all 0.4s ease;
	position: absolute;
}

.list-move {
	transition: all 0.3s ease;
}

.switch-enter-from,
.switch-leave-to {
	opacity: 0;
	transform: translateY(20px);
}

.switch-enter-active,
.switch-leave-active {
	transition: all 0.2s ease;
}
</style>
