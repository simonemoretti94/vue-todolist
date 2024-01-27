<script>
export default {
  name: 'app',
  data() {
    return {

      /* main info array and utilities */
      toDo: [
        {
          text: 'Clean the apartment',
          done: true,
          link: 'https://media.istockphoto.com/id/1317558249/it/video/soggiorno-con-divano.jpg?s=640x640&k=20&c=idK6AzYrp5wC40fTLjqJE-rjHlQ2ejMc7V4KYDeCdUo=',
          keyword: 'apartment',
        },
        {
          text: 'Fixing the bike\'s chain',
          done: true,
          link: 'https://c1.wallpaperflare.com/preview/577/279/665/night-park-street-bike.jpg',
          keyword: 'bike',
        },
        {
          text: 'Buying groceries',
          done: false,
          link: 'https://www.gottaoffer.com/blog/wp-content/uploads/2019/07/img04-2x-1330x680-c-1024x524.jpg',
          keyword: 'groceries',
        },
        {
          text: 'Ask the boss for a raise',
          done: true,
          link: 'https://th.bing.com/th/id/R.9e13ce6e8e58b9ff09d410f7fcb7ee1d?rik=lHpUPeouz%2b4z8A&pid=ImgRaw&r=0',
          keyword: 'salary raise',
        },
      ],

      /* footer info array and utilities*/

      newTask: 'Write here the language',
      stringTasks: [
        'Learn sass',
        'Learn vue',
        'Learn php',
        'Learn laravel',
      ],

      errorMsg: false,

    }
  },
  methods: {

    /* main functions */

    removeItem(index) {

      console.log('index: ', index, ' element: ', this.toDo[index]);

      this.toDo.splice(index, 1);
    },

    changeVal(index) {
      if (this.toDo[index].done === true) {
        this.toDo[index].done = false;
      }
      else {
        this.toDo[index].done = true;
      }
    },

    /* footer functions */

    removeTask(taskIndex) {

      console.log('removeTaskF says: removing task ', taskIndex, ' typeof taskindex: ', Boolean(taskIndex));

      this.tasks.splice(taskIndex, 1);
    },

    addTask() {
      console.log('Add task: ', this.newTask);

      if (this.newTask.length >= 3) {
        this.newTask = this.newTask.toLowerCase()
        console.log('newTask type: ', this.newTask);
        this.stringTasks.unshift('Learn ' + this.newTask);

        this.newTask = '';
        if (this.errorMsg !== false) {
          this.errorMsg = false;
        }
      }
      else {
        this.errorMsg = 'The input word must match at least three characters.';
        console.log(this.errorMsg);
      }

    },

    removeTaskLi(index) {
      this.stringTasks.splice(index, 1);
    }
  },
};
</script>

<template>
  <header class="d-flex justify-content-center">
    <h1 class="display-1 text-center text-white my-2 p-1">Vue-todolist</h1>

  </header>

  <main class="d-flex flex-column  justify-content-evenly align-items-center">

    <div id="main_container" class="container col-7 d-flex flex-row flex-wrap justify-content-center  align-items-center">

      <div v-if="toDo.length > 0" id="div_card" class="col-5 m-auto my-2 d-flex flex-row border rounded-2"
        v-for="(element, index) in toDo">
        <div class="col-6">
          <img class="p-2" :src="element.link" :alt="element.keyword + ' img'">
        </div>
        <div class="col-6 d-flex flex-column ">
          <div class="d-flex flex-row">
            <p :class="{ 'text-decoration-line-through': !element.done }"
              class="col-10 text-white text-center flex-wrap ">
              {{
                element.text }}</p>
            <span id="span_1" class="col text-center text-danger" v-on:click="removeItem(index)">X</span>
          </div>
          <div class="col-12 d-flex justify-content-center align-items-end"><span
              :class="toDo[index].done ? 'bg-success' : 'bg-danger '" class="btn text-white border rounded-2 mb-1"
              v-on:click="changeVal(index)">{{
                element.done }} </span></div>
        </div>
      </div>
      <h2 v-else class="text-center my-2 text-white">You've done well! 😎</h2>

    </div>



  </main>

  <footer class="d-flex flex-column justify-content-center align-items-center">

    <div class="container my-4">
      <div class="card p-3 bg-dark ">
        <div class="input-group d-flex justify-content-center mb-2">
          <input name="input_task" v-model="newTask" type="text" class="border rounded-1 text-center text-white"
            @keyup.enter="addTask()">
          <button class="btn btn-ligth text-white border rounded-2 ms-1" @click="addTask()">Add task</button>
        </div>

        <p v-if="errorMsg" class="my-2">{{ errorMsg }}</p>

        <div class="ul-list-group list-unstyled d-flex flex-row justify-content-evenly flex-wrap p-1 border-special">
          <li v-if="stringTasks.length > 0" v-for="(string, index) in stringTasks"
            class="col-5 text-center text-white my-1">{{ string
            }} <span v-on:click="removeTaskLi(index)" class="ms-1 text-center text-danger">X</span></li>
          <h2 v-else class="my-2 text-center text-warning">No languages availables! 😎</h2>
        </div>

      </div>
    </div>

  </footer>
</template>

<style>
header {
  border-bottom: 2px solid white;
}

#div_card p {
  border-bottom: 1px dashed white;
}

img {
  height: 100px;
  width: 100px;
  max-width: 100px;
  max-height: 110px;
  filter: drop-shadow(3px 3px 5px rgb(80, 80, 80));
}

#span_1 {
  font-style: italic;
  font-weight: bold;
}
</style>
