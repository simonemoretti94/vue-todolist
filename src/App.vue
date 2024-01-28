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

      newTask: 'Write here a task',
      stringTasks: [
      ],

      errorMsg: false,
      counterAddTask: 0,

    }
  },
  methods: {

    /* main functions */

    //remove the item from toDo array
    removeItem(index) {

      console.log('index: ', index, ' element: ', this.toDo[index]);

      this.toDo.splice(index, 1);
    },

    //change the boolean value into toDo chosen element
    changeVal(index) {
      if (this.toDo[index].done === true) {
        this.toDo[index].done = false;
      }
      else {
        this.toDo[index].done = true;
      }
    },

    /* footer functions */

    //adds input task into toDo array
    addTask() {
      console.log('Add task: ', this.newTask);

      if (this.newTask.length >= 3 && this.newTask !== 'Write here a task') {
        this.newTask = this.newTask.toLowerCase();
        console.log('newTask type: ', this.newTask);
        this.counterAddTask++;

        const tempObj = {
          text: this.newTask,
          done: false,
          link: `https://source.unsplash.com/random/200x200?sig=${this.counterAddTask}`,
          keyword: '',
        }

        this.stringTasks.unshift(this.newTask);
        this.toDo.unshift(tempObj);

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

    //remove li element task from footer's container
    removeTaskLi(index) {
      this.stringTasks.splice(index, 1);
    }
  },
};
</script>

<template>
  <header class="d-flex justify-content-center">

    <!-- header writings -->
    <h1 class="display-1 text-center text-white my-2 p-1">Vue-todolist</h1>

  </header>

  <main class="d-flex flex-column  justify-content-evenly align-items-center">

    <!-- container that binds together first block of cards -->
    <div id="main_container" class="container col-7 d-flex flex-row flex-wrap justify-content-center  align-items-center">

      <!-- root of iteration in toDo array -->
      <div v-if="toDo.length > 0" id="div_card" class="col-5 m-auto my-2 d-flex flex-row border rounded-2"
        v-for="(element, index) in toDo">

        <!-- card's left column -->
        <div class="col-5 d-flex justify-content-center align-items-center">
          <img class="p-2" :src="element.link" :alt="element.keyword + ' img'">
        </div>

        <!-- card's right column -->
        <div class="col-7 d-flex flex-column ">

          <!-- top row -->
          <div class="d-flex flex-row">
            <p :class="{ 'text-decoration-line-through': !element.done }, !element.done ? 'text-primary' : 'text-white'"
              class="col-10 text-center flex-wrap ">
              {{
                element.text }}</p>
            <span id="span_1" class="col text-center text-danger" v-on:click="removeItem(index)">X</span>
          </div>

          <!-- bottom row -->
          <div class="col-12 d-flex justify-content-center align-items-end"><span
              :class="toDo[index].done ? 'bg-success' : 'bg-danger '" class="btn text-white border rounded-2 mb-1"
              v-on:click="changeVal(index)">{{
                element.done }} </span>
          </div>

        </div>

      </div>

      <!-- h2 text shown if main container is empty of todo tasks -->
      <h2 v-else class="text-center my-2 text-white">You've done well! 😎</h2>

    </div>



  </main>

  <footer class="d-flex flex-column justify-content-center align-items-center">

    <!-- container that binds together second block of cards -->
    <div class="container my-4">

      <!-- card structure -->
      <div class="card p-3 bg-dark ">

        <!-- input group that binds together input and button  -->
        <div class="input-group d-flex justify-content-center mb-2">

          <!-- btn that accepts string task and widens in a specific condition -->
          <input name="input_task" v-model="newTask" type="text" :class="newTask.length > 18 ? 'col-6' : ''"
            class="border rounded-1 text-center text-white" @keyup.enter="addTask()">

          <!-- button that submits the input's string of the previous tag above -->
          <button class="btn btn-ligth text-white border rounded-2 ms-1" @click="addTask()">Add task</button>

        </div>

        <!-- text that shows up in case newTask's lenght is equal or lesser than 3 whole numbers -->
        <p v-if="errorMsg" class="my-2 text-white">{{ errorMsg }}</p>

        <!-- ul containing generated list items -->
        <div class="ul-list-group list-unstyled d-flex flex-row justify-content-evenly flex-wrap p-1 border-special">

          <!-- list items generated from iteration in stringTasks arry -->
          <li v-if="stringTasks.length > 0" v-for="(string, index) in stringTasks"
            class="col-5 text-center text-white flex-wrap my-1">{{ string
            }}

            <!-- span cross that calls removeTaskLi function and remove it from stringTasks array -->
            <span id="span_2" v-on:click="removeTaskLi(index)" class="ms-1 text-center text-danger">X</span>
          </li>

          <!-- h2 text shown in case of footer container is empty of string tasks -->
          <h2 v-else class="my-2 text-center text-warning">No others tasks on the list,<br> Great! 😉</h2>
        </div>

      </div>
    </div>

  </footer>
</template>

<style>
/* header and main */

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

#span_1,
#span_2 {
  font-style: italic;
  font-weight: bold;
  cursor: pointer;
}

/* footer */

ul>li {
  border: 1px rgb(13, 13, 119) dashed;
  box-shadow: 2px 2px white;
  filter: drop-shadow(2px 3px 4px rgb(106, 199, 230));

  padding: .25rem;
  font-size: large;
  font-weight: 700;
  font-style: oblique;
  word-spacing: 4px;
  letter-spacing: 1.5px;
}

span>svg {
  fill: red;
}

.border-special {
  border: solid 1px white;
  box-shadow: 2px 2px grey;
  filter: drop-shadow(1.5px 1.5px 2px black);
}

.border-special:hover {
  transform: scale(.9);
  border-radius: 15px;
}

input {
  min-height: 40px;
  min-width: 150px;
  font-size: small;
}

.ul-list-group>li {
  filter: drop-shadow(1.5px 1.5px 1.5px rgb(95, 205, 241));
  border: dashed .5px white;
  border-radius: 10px;
  margin: auto .5rem auto .5rem;

}
</style>
