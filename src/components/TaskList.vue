<template>
    <div class="tasks">
        <table>
            <thead>
                <tr>
                    <td colspan="3">
                        <input type="text" maxlength="30" placeholder="New task" v-model="name">
                        <button v-on:click="addTask">+</button>
                    </td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="task in tasklist" :key="task.id">
                    <td><input type="checkbox" v-model="task.checked" v-on:click="checkTask(task)"></td>
                    <td>{{ task.name }}</td>
                    <td><img class="trash" src="../assets/imgs/trash.png" alt="Delete" v-on:click="removeTask(task.id)"></td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: "TaskList",
    data () {
        return {
            name: "",
            checked: false,
            tasklist: []
        }
    },
    methods: {
        async loadData () {
            const response = await axios.get('http://localhost:3000/tasks/');
            this.tasklist = response.data;
        },
        async addTask () {
            if (this.name) {
                const response = await axios.post('http://localhost:3000/tasks/', { name: this.name, checked: false });
                if (response.status == 201) {
                    this.loadData();
                    this.name = "";
                }
            }
        },
        async removeTask (id) {
            const response = await axios.delete('http://localhost:3000/tasks/'+id);
            if (response.status == 200)
                this.loadData();
        },
        async checkTask (task) {
            const response = await axios.put('http://localhost:3000/tasks/'+task.id, { name: task.name, checked: !task.checked });
            if (response.status == 200)
                this.loadData();
        }
    },
    mounted () {
        this.loadData();
    }
}
</script>

<style>
.tasks {
    margin: auto; padding: 20px;
    background-color: #fff;
    border-radius: 10px;
}

.tasks table {
    margin: 0; padding: 0;
    width: 400px;
}

.tasks tbody {
    margin-top: 20px;
}

.tasks table td {
    width: fit-content; height: 40px;
    text-align: center; vertical-align: middle;
}

.tasks input[type="checkbox"] {
    -webkit-appearance: none; appearance: none; margin: 0;
    font: inherit; color: currentColor;
    width: 1.15em; height: 1.15em;
    border: 1px solid currentColor;
    border-radius: 50%; cursor: pointer;
}

.tasks input[type="checkbox"]:checked {
    border-color: aqua; background-color: aqua;
}

.tasks thead td input {
    width: 300px; height: 100%;
    border-radius: 30px; border: none transparent;
    padding-left: 20px; margin: auto;
    background-color: #efefef;
    font-size: .9rem; outline: none;
}

.tasks thead td button {
    width: 40px; height: 100%;
    margin-left: 20px; padding: 0;
    border-radius: 50%; border: none transparent;
    background-color: aquamarine;
    cursor: pointer; font-size: 1.4rem;
}

.tasks thead td button:hover {
    background-color: lightgreen;
}

.trash {
    padding: 0; margin: 0;
    cursor: pointer;
}

@media (max-width: 450px) {
    .tasks table {
        width: 300px;
    }

    .tasks thead td input {
        width: fit-content;
    }
}
</style>