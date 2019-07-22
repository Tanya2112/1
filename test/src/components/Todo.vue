<template>
    <div class="todo">
        <div class="lists">
            <select v-model="selectedItem" @change="ChoiceSelectedItem">
                <option value="1">Неисполненные</option>
                <option value="2">Исполненные</option>
                <option value="3">Все</option>
            </select>
                <div v-show="taskFilter(index, task.color)" :class="{'white': task.color === 0, 'green': task.color === 1, 'grey': task.color === 2,
                'border': Choice === index}"
                     v-for="(task, index) in SortArr" class="list-item" :key="index" @click="choiceCase(index)">
                    <span>{{task.title}}</span>
                    <button class="remove-item" @click="removeTask(index)">&times</button>
                </div>
        </div>
        <div class="bot">
            <hr>
            <input type="text" placeholder="Введите название списка" v-model="newTask.title">
            <button class="buttons" @click="addTask">Добавить</button>
        </div>
    </div>

</template>

<script>
    export default {
        name: "Todo",
        props: ['todoTask', 'newTask', 'Choice'],
        data() {
            return {
                selectedItem: 1,
            }
        },
        methods: {
            addTask() {
                this.$emit('AddTask', this.newTask.title)
            },
            removeTask(index) {
                this.$emit('RemoveTask', index);
            },
            choiceCase(index) {
                this.$emit('ChoiceCase', index);
            },
            ChoiceSelectedItem() {
                this.$emit('ChoiceSelectedItem')

            },
            taskFilter(index, color) {
                if(this.selectedItem == 1) {
                    return color === 0 || color === 1
                }
                if(this.selectedItem == 2) {
                    return color === 2
                }
                if(this.selectedItem == 3) {
                    return true
                }
            }
        },
        computed: {
            SortArr() {
                function f(a,b) {
                    if (a.title.toLowerCase() < b.title.toLowerCase()) {
                        return -1;
                    }
                    if (a.title.toLowerCase() > b.title.toLowerCase()) {
                        return 1;
                    }
                    return 0;
                }
                return this.todoTask.sort(f)
            }
        }
    }
</script>

<style scoped>
    select {
        width: 90%;
        margin-left: 6%;
        margin-top: 5%;
        margin-bottom: 5%;
        border-radius: 10px;
        height: 35px;
    }

    .todo {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        position: relative;
        width: 25%;
        height: 100%;
        border-right: 1px solid #2c3e50;
        border-radius: 15px;
        top: 0;
    }

    .lists {
        height: 84%;
        margin: 0;
        overflow: auto;
    }

    .list-item{
        width: 80%;
        position: relative;
        list-style-type: none;
        border: 1px solid grey;
        border-radius: 15px;
        padding: 10px 0 10px 15px;
        margin-left: 7%;
        margin-bottom: 1%;
    }

    /*.list-item:hover {*/
    /*    border: 1px solid #167500;*/
    /*}*/

    .bot{
        text-align: center;
        position: absolute;
        bottom: 2%;
    }

    input[type="text"]{
        width: 90%;
        border-radius: 5px;
        margin-bottom: 2%;
        height: 30px;
    }

    .buttons {
        width: 93%;
        border-radius: 5px;
        height: 35px;
        background-color: #c9ffbd;
    }

    .remove-item {
        position: absolute;
        background: none;
        border: none;
        right: 2px;
        top: 2px;
    }

    .white {
        background-color: white;
    }

    .green {
        background-color: #e6ffe0;
    }

    .grey {
        background-color: #e6e5e5;
    }

    .border {
        border: 2px solid #167500;
    }

</style>