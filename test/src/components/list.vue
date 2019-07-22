<template>
    <div class="list">
        <div class="list-item">
            <p v-show="TaskList.lists.length < 1 && Tasks.length > 0">
                Добавьте дело.
            </p>
            <ul>
                <li :class="{'add': task.done, 'hidden': Active}" v-for="(task, index) in TaskList.lists" :key="task.title">
                    <div class="start">
                        <input type="checkbox" :checked="task.done" @click="doneList(index)">
                        <span class="item-title">{{task.title}}</span>
                    </div>

                    <div class="end">
                        <span class="red" v-if="task.urgently"></span>
                        <span class="item-date">{{formatDate(task.date)}}</span>
                        <button class="remove-item" @click="removeList(index)">&times</button>
                    </div>
                </li>
            </ul>
        </div>
        <div class="bott">
            <hr>
            <input type="text" placeholder="Добавить дело" v-model="newTitle">
            <input type="checkbox" id="one" v-model="status">
            <label for="one">Срочное</label>
            <button @click="addList()" class="buttonBottom">Добавить</button>
        </div>
    </div>
</template>

<script>
    export default {
        name: "list",
        props: ['TaskList', 'newList', 'Tasks', 'Active'],
        data () {
            return {
                newTitle: '',
                status: false,
                done: false
            }
        },
        methods: {
            addList() {
                 this.$emit('AddList', this.newTitle, this.status);
                 this.newTitle= ''
            },
            formatDate(d) {
                if(d !== undefined) {
                    let date = d.getDate();
                    if(date < 10) {
                        date = '0' + String(date);
                    }
                    let month = d.getMonth() + 1;
                    if(month < 10) {
                        month = '0' + String(month);
                    }
                    const year = d.getFullYear();
                    let minutes = d.getMinutes();
                    if(minutes < 10) {
                        minutes = '0' + String(minutes);
                    }
                    let hour = d.getHours();
                    if(hour < 10) {
                        hour = '0' + String(hour);
                    }
                    return (date + "-" + month + "-" + year + ' ' + hour + ':' + minutes);
                }
            },
            removeList(index) {
                this.$emit('RemoveList', index);
                if(this.TaskList.lists.length === 0) {
                    this.$emit('WhiteColor')
                }
            },
            doneList(index) {
                this.$emit('DoneList', index);

                if(this.TaskList.lists.length === 0) {
                    this.$emit('WhiteColor')
                }
                else {
                    let Check = 0;
                    for (let i = 0; i < this.TaskList.lists.length; i++) {
                        if(this.TaskList.lists[i].done) {
                            Check++;
                        }
                    }

                    if(Check === this.TaskList.lists.length) {
                        this.$emit('GreyColor')
                    }
                    else {
                        this.$emit('GreenColor')
                    }
                }
            },
        }
    }
</script>

<style scoped>
    p {
        text-align: center;
    }

    .list {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        width: 75%;
        height: 100%;
        border-left: 1px solid #2c3e50;
        border-radius: 15px;
        position: relative;
    }

    .start {
        display: flex;
        align-items: center;
    }

    ul {
        width: 88%;
    }

    li {
        display: flex;
        justify-content: space-between;
        list-style-type: none;
        border: 1px solid grey;
        border-radius: 15px;
        padding: 10px 0 10px 15px;
        margin-bottom: 1%;
    }

    .list-item {
        height: 85%;
        overflow: auto;
    }

    .remove-item {
        background: none;
        margin-top: -15px;
        border: none;
        outline: none;
        margin-left: 7px;
    }

    .end {
        display: flex;
        align-items: center;
        justify-content: flex-end;
        width: 35%;
    }

    .red {
        padding: 5px;
        border:1px solid black;
        background-color: red;
        border-radius: 100%;
        margin: auto 5%;
    }

    .bott {
        padding-bottom: 4%;
    }

    input[type="text"] {
        width: 50%;
        border-radius: 5px;
        height: 30px;
        margin-left: 5%;
        margin-top: 2.8%;
    }

    label {
        margin-right: 10%;
    }

    .buttonBottom {
        width: 15%;
        border-radius: 5px;
        height: 35px;
        background-color: #c9ffbd;
    }

    .add {
        background-color: #e6ffe0;
    }

    .hidden {
        display: none;
    }

</style>