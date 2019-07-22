<template>
  <div id="app">
    <Todo :todoTask="tasks" :Choice="choiceCases" :newTask="newTask" @AddTask="addTasks" @RemoveTask='removeTasks'
          @ChoiceCase="choiceCase" @ChoiceSelectedItem="ChoiceSelectedItem" ></Todo>
    <list :TaskList="tasks[choiceCases]" :Tasks="tasks" :newList="newTask" @AddList="addLists" @RemoveList="removeLists"
          @DoneList="DoneTask" @WhiteColor="WHITEcolor" @GreyColor="GREYcolor" @GreenColor="GREENcolor" :Active="active"></list>
  </div>
</template>

<script>
import Todo from "./components/Todo";
import List from "./components/list";

export default {
  name: 'app',
  data () {
    return {
      active: false,
      choiceCases: 0,
      newTask: {
        color: 0,
        title: '',
        lists: [
          {
            title: '',
            date: new Date,
            urgently: false,
            done: false
          }
        ]
      },
      tasks: [
        {
          color: 1,
          title:'Список 1',
          lists: [
            {
              title: 'Подзадача списка 1',
              date: new Date(),
              urgently: false,
              done: false
            }
          ]
        },
        {
          color: 1,
          title:'Список 2',
          lists: [
            {
              title: 'Подзадача списка 2',
              date: new Date(),
              urgently: false,
              done: false
            }
          ]
        }
      ]
  }},
  methods: {
    addTasks() {
      let checkSame = false;
      for(let i = 0; i < this.tasks.length; i++) {
        if(this.newTask.title === this.tasks[i].title) {
          checkSame = true
        }
      }

      if(checkSame) {
        alert("Такой список дел уже существует")
      }
      else {
        if(this.newTask.title !== '') {
          this.tasks.push({color: 0, title: this.newTask.title, lists: []});
          alert('Список дел ' + this.newTask.title +' добавлен')
        }
        else {
          alert("Вы ввели пустую строку")
        }
      }
      this.newTask.title = ''
    },
    removeTasks(index) {
      const per = this.tasks.length;
      if(per > 1) {
        const quest = confirm('Вы действительно хотите удалить список дел?');
        if (quest) {
          // this.tasks[index].lists.splice(0, this.tasks[index].lists.length);
          this.choiceCases = 0;
          this.tasks.splice(index, 1);
        }
      }
    },
    choiceCase(index) {
        this.choiceCases = index;
    },
    addLists(text, status) {
      let checkSame2 = false;
      for(let i = 0; i < this.tasks.length; i++) {
        for(let j = 0; j < this.tasks[i].lists.length; j++) {
          if (text === this.tasks[i].lists[j].title) {
            checkSame2 = true
          }
        }
      }
      if(checkSame2) {
        alert('Дело с таким названием уже существует')
      }
      else {
        if(text !== '') {
          this.tasks[this.choiceCases].lists.unshift({
              title: text,
              date: new Date(),
              urgently: status,
              done: false
          });
          this.tasks[this.choiceCases].color = 1;
          alert('Дело ' + text +' добавлено')
        }
        else {
          alert("Вы ввели пустую строку")
        }
      }
    },
    removeLists(index) {
      const quest = confirm('Вы действительно хотите удалить это дело?');
      if(quest) {
        this.tasks[this.choiceCases].lists.splice(index,1);
      }
    },
    DoneTask(index) {
      this.tasks[this.choiceCases].lists[index].done = !this.tasks[this.choiceCases].lists[index].done;
    },
    WHITEcolor() {
      this.tasks[this.choiceCases].color = 0;
    },
    GREENcolor() {
      this.tasks[this.choiceCases].color = 1;
    },
    GREYcolor() {
      this.tasks[this.choiceCases].color = 2;
    },
    ChoiceSelectedItem() {
      this.active = false;
    }
  },
  mounted (){
    //console.log(this.tasks[0].lists.length)

  },
  components: {
      Todo,
      List
    }
}
</script>

<style>
#app {
  display: flex;
  flex-wrap: nowrap;
  justify-content: space-between;
  height: 600px;
  width: 80%;
  border: 1px solid #2c3e50;
  border-radius: 15px;
  margin:auto;
}
</style>
