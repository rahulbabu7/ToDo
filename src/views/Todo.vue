<template>
    <div class="min-h-screen flex justify-center items-center bg-gray-900">
      <div class="bg-white rounded shadow-md p-6 w-full sm:w-96">
        <h1 class="text-3xl font-semibold mb-4 text-center text-blue-600">
          To-Do App
        </h1>
        <form @submit.prevent="addTodo" class="mb-4">
          <label class="block mb-1 text-sm font-medium text-gray-700">
            Enter a task:
          </label>
          <div class="flex">
            <input
              v-model="todo"
              type="text"
              class="flex-1 border rounded py-2 px-3 text-gray-800 focus:outline-none focus:ring focus:border-blue-300"
              placeholder="Add a new task"
            />
            <button
              type="submit"
              class="ml-2 bg-blue-500 text-white py-2 px-4 rounded hover:bg-blue-600 transition"
            >
              Add
            </button>
          </div>
          <p class="text-red-500 mt-1" v-if="errormsg">Please enter a task.</p>
        </form>
        <section>
          <ul>
            <li
              v-for="item in todoList"
              :key="item.id"
              class="bg-gray-100 py-2 px-3 mb-1 rounded-md shadow-sm flex items-center justify-between"
            >
              <span class="text-gray-800">{{ item.text }}</span>
              <button
                class="text-red-600 hover:text-red-800"
                @click="removeTodo(item.id)"
              >
                Remove
              </button>
            </li>
          </ul>
        </section>
      </div>
    </div>
  </template>
  
  <script setup>
  
  import { ref ,onMounted } from 'vue';
  import {db,auth} from '../firebase/firebase';
  const todo = ref('');
  const todoList = ref([]);
  const errormsg = ref(false);
  

  async function fetchTodos() {
  const user = auth.currentUser;
  if (user) {
    const userId = user.uid;
    const todosSnapshot = await db.collection('Todos').where('userId', '==', userId).get();
    todoList.value = todosSnapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
  }
}

// Watch for changes in Firestore collection
onMounted(fetchTodos);

// fetchTodos();


async function addTodo() {
  const user = auth.currentUser;
  
  if (user && todo.value !== '') {
    const userId = user.uid;
    await db.collection("Todos").add({
      userId: userId,
      text: todo.value
    });

    todo.value = '';
    errormsg.value = false;
    fetchTodos();
  } else {
    errormsg.value = true;
    todo.value = '';
    fetchTodos();
  }
}

async function removeTodo(id) {
  const user = auth.currentUser;

  if (user) {
    const todoRef = db.collection("Todos").doc(id);
    const todoDoc = await todoRef.get();

    if (todoDoc.exists) {
      const todoData = todoDoc.data();
      
      if (todoData.userId === user.uid) {
        await todoRef.delete();
        fetchTodos();
      } else {
        console.log("You don't have permission to delete this todo.");
      }
    } else {
      console.log("Todo does not exist.");
    }
  } else {
    console.log("User not authenticated.");
  }
}
/*
async function addTodo() {
    if (todo.value !== '') {
      await db.collection("Todos").add({
        text:todo.value
      });
      todo.value = '';
      errormsg.value = false;
      fetchTodos();
    }
     else {
      errormsg.value = true;
      todo.value = '';
      fetchTodos();

    }
  }
 async function removeTodo(id){
    await db.collection("Todos").doc(id).delete();
    fetchTodos();

  }
  */
  </script>
  
  <style>
  /* Mobile Styles */
  @media (max-width: 639px) {
    .sm\:w-96 {
      width: 100%; /* Use full width on mobile */
    }
  }
  
  /* Tablet Styles */
  @media (min-width: 640px) and (max-width: 767px) {
    .sm\:rounded-full {
      border-radius: 50%; /* Make input rounded on tablet */
    }
  }
  
  /* Desktop Styles */
  @media (min-width: 1024px) {
    .lg\:hover\:bg-blue-600:hover {
      background-color: #2563EB; /* Change button color on desktop hover */
    }
  }
  </style>
  