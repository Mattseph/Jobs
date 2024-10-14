<!-- OPTION API -->
<script>
export default {
  data() {
    return {
      name: "Ela Mae Jetigan",
      status: "actiasdasdve",
      tasks: ["one", "two", "three", "four", "five"],
      link: "https://mattseph.github.io/portfolio/",
    };
  },

  methods: {
    toggleStatus() {
      if (this.status === "active") {
        this.status = "pending";
      } else if (this.status === "pending") {
        this.status = "inactive";
      } else {
        this.status = "active";
      }
    },
  },
};
</script>

<!-- COMPOSITION API-->
<!-- lONG VERSION

<script>

import { ref } from 'vue';

export default {
  setup() {
    const name = ref("Matthew Joseh F. Bilaos");
    const status = ref("Active");
    const tasks = ref(["one", "two", "three", "four", "five"]);
    const link = ref("https://mattseph.github.io/portfolio/");

    const toggleStatus = () => {
      if (status.value === "active") {
        status.value = "pending";
      } else if (status.value === "pending") {
        status.value = "inactive";
      } else {
        status.value = "active";
      }
    };

    return {
      name,
      status,
      tasks,
      link,
      toggleStatus,
    }
  },
};
</script> -->

<!-- Short Version -->
<script setup>
import { ref, onMounted } from "vue";

const name = ref("Matthew Joseh F. Bilaos");
const status = ref("Active");
const tasks = ref(["one", "two", "three", "four", "five"]);
const link = ref("https://mattseph.github.io/portfolio/");
const newTask = ref("");

const toggleStatus = () => {
  if (status.value === "active") {
    status.value = "pending";
  } else if (status.value === "pending") {
    status.value = "inactive";
  } else {
    status.value = "active";
  }
};

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1); // remove 1 item based on the index
};

onMounted(async () => {
  try {
    // Fetch from endpoint
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");

    // Get the data
    const data = await response.json();

    //Map the data and get only the title
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log("Error Fetching Tasks");
  }
});
</script>

<template>
  <h2>Hello Vue</h2>
  <p v-if="status === 'active'">I love Ela Mae</p>
  <p v-else-if="status === 'pending'">I love Ela Mae so muchhh</p>
  <p v-else>I love Ela Mae so so muchh</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Task</label>
    <input type="text" name="newTask" id="newTask" v-model="newTask" />
    <button type="submit">Add Task</button>
  </form>

  <p>{{ newTask }}</p>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>
        {{ task }}
      </span>
      <button @click="deleteTask(index)">X</button>
    </li>
  </ul>

  <a :class :href="link">Portfolio</a>

  <!-- Either v-on:click or @click -->
  <button @click="toggleStatus">Toggle Status</button>
</template>

<style scoped>
h2 {
  color: green;
}
</style>

<template>
  <h2>Hello Vue</h2>
  <p v-if="status === 'active'">I love Ela Mae</p>
  <p v-else-if="status === 'pending'">I love Ela Mae so muchhh</p>
  <p v-else>I love Ela Mae so so muchh</p>

  <ul>
    <li v-for="task in tasks" :key="task">{{ task }}</li>
  </ul>

  <a :class :href="link">Portfolio</a>

  <!-- Either v-on:click or @click -->
  <button @click="toggleStatus">Toggle Status</button>
</template>

<style scoped>
h2 {
  color: green;
}
</style>
