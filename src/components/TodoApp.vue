<template>
    <div class="container">
        <h2 class="text-center mt-5">My Vue Todo App</h2>

        <!-- input -->
        <div class="d-flex">
            <input
                type="text"
                placeholder="Enter task ... "
                class="form-control"
                v-model="newTask"
            />
            <button
                class="btn btn-warning rounded-1"
                @click.prevent="submitTask()"
            >
                Submit
            </button>
        </div>

        <!-- Table -->

        <table class="mt-5 table table-bordered text-center">
            <thead>
                <tr>
                    <th scope="col">Task</th>
                    <th scope="col">Status</th>
                    <th scope="col">#</th>
                    <th scope="col">#</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index">
                    <th style="width: 400px" scope="row">
                        <span :class="{ finished: task.status === 'finished' }">
                            {{ task.name }}
                        </span>
                    </th>
                    <td style="width: 140px">
                        <div @click.prevent="changeStatus(index)">
                            <span
                                class="pointer"
                                :class="{
                                    'text-danger': task.status === 'to-do',
                                    'text-warning':
                                        task.status === 'in-processing',
                                    'text-success': task.status === 'finished',
                                }"
                            >
                                {{ task.status }}
                            </span>
                        </div>
                    </td>
                    <td>
                        <div @click.prevent="editTask(index)">
                            <span class="fa fa-pen"></span>
                        </div>
                    </td>
                    <td>
                        <div @click.prevent="deleteTask(index)">
                            <span class="fa fa-trash"></span>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    data() {
        return {
            newTask: '',
            editIndexTask: null,
            availableStatus: ['to-do', 'in-processing', 'finished'],
            tasks: [
                {
                    name: 'Learn VueJS',
                    status: 'in-processing',
                },
            ],
        };
    },
    methods: {
        submitTask() {
            if (this.newTask.length === 0) return;

            if (this.editIndexTask === null) {
                this.tasks.push({
                    name: this.newTask,
                    status: 'to-do',
                });
            } else {
                this.tasks[this.editIndexTask].name = this.newTask;
            }
            this.newTask = '';
        },
        deleteTask(index) {
            this.tasks.splice(index, 1);
        },
        editTask(index) {
            this.newTask = this.tasks[index].name;
            this.editIndexTask = index;
        },
        changeStatus(index) {
            let newIndex = this.availableStatus.indexOf(
                this.tasks[index].status
            );
            if (++newIndex > 2) newIndex = 0;
            this.tasks[index].status = this.availableStatus[newIndex];
        },
    },
};
</script>

<style>
.pointer {
    cursor: pointer;
}
.finished {
    text-decoration: line-through;
}
</style>
