<template>
    <div class="col-6 offset-3">
        <h1 class="text-center bg-success text-white p-3">Todo List</h1>
        <div class="container">
            <div class="row my-3">
                <div class="col">
                    <input type="text" class="form-control" v-model="newTask" v-on:keyup.enter="addTasks()">
                </div>
                <div class="col">
                    <input type="button" value="Add" class="btn btn-secondary" v-on:click="addTasks()">
                </div>
                <div class="col">
                    <button class="btn btn-warning" @click="deleteTask()">Delete Tasks</button>
                </div>
            </div>
            <div class="" v-if="filterTask.length > 0">
                <div class="row">
                    <div class="col  mb-4">Tasks</div>
                    <div class="col-1 mb-4">Done</div>
                </div>
                <div class="row" v-for="(task, index) in filterTask" :key="index">
                    <div class="col" :class="task.done ? 'delete' : ''">
                        {{ task.action }}</div>
                    <div class="col-1">
                        <input type="checkbox" v-model="task.done" id="">
                    </div>
                </div>
            </div>
            <div class="alert alert-warning text-center" v-else>There is no data</div>
            <div class="bg-danger row mt-3 text-center  text-white py-2 ">
                <h5 class="col">Hide Completed Task</h5>
                <input class="col " type="checkbox" v-model="hideCompleted" id="" style="transform: scale(0.5);">
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "App",
    data: () => ({
        title: "Techno",
        hideCompleted: false,
        newTask: '',
        tasks: [],
    }),
    computed: {
        filterTask() {
            return this.hideCompleted ?
                this.tasks.filter(v => !v.done) :
                this.tasks;
        },
    },
    methods: {
        addTasks() {
            if (this.newTask === "") {
                alert("Please add the task");
            } else {
                this.tasks.push({
                    action: this.newTask,
                    done: false,
                });
                this.storeData();
                this.newTask = "";
            }
        },
        deleteTask() {
            this.tasks = this.tasks.filter((v) => !v.done);
            this.storeData();
        },
        storeData() {
            localStorage.setItem("myLocalTasks", JSON.stringify(this.tasks));
        }

    },
    created() {
        let data = localStorage.getItem("myLocalTasks");
        if (data !== null) {
            this.tasks = JSON.parse(data);
        }
    },

};
</script>

<style>
.delete {
    text-decoration: line-through;
}
</style>

