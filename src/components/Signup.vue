<template>
    <div class="flex flex-col min-h-screen bg-gray-700">
       <main class="flex-grow flex items-center justify-center">
     <section class="p-8 w-96">
       <h2 class="text-2xl text-white mb-4">Create Account</h2>
       <form class="mb-4">
         <input v-model="email"  type="text" placeholder="Email" class="block w-full p-2 border rounded border-gray-300 focus:outline-none focus:border-blue-500">
         <input v-model="password" type="password" placeholder="Password" class="mt-2 block w-full p-2 border rounded border-gray-300 focus:outline-none focus:border-blue-500">
       
       <button @click="register" class="w-full bg-green-500 hover:bg-green-600 text-white font-semibold py-2 px-4 rounded focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-400 mt-4">
         Continue
       </button>
   </form>
       <h5 class="mt-4 text-white">
        Already have an account? <a href="#" class="text-green-900 hover:underline no-underline">Login</a>
      </h5>
   
       <!-- <button class="w-full bg-white hover:bg-white text-black py-2 px-4 rounded focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-400 mt-6">
         Continue with Google
       </button> -->
     </section>
   </main>
   <footer class="mt-auto mb-8 text-center flex justify-center gap-x-6">
     <button class="text-neutral-500">Terms of use</button> 
     <button class="text-neutral-500">Privacy Policy</button> 
   </footer>
    </div>

 </template>
 
 <script setup>
import firebase from 'firebase/app';
import 'firebase/auth';
 import {ref} from 'vue';
 const email = ref("");
 const password = ref("");


 function register(e){
 e.preventDefault();
 const trimmedEmail = email.value.trim();
  const trimmedPassword = password.value.trim();
 firebase
 .auth()
 .createUserWithEmailAndPassword(trimmedEmail,trimmedPassword)
  .then(user =>{
    alert(`user created ${ user.user.email}` );  //user.user. The double user.user might look a bit unusual, but it suggests that user is an object that has a property named user (which might be confusing in its own right).
  },
  err =>{
    alert(err.message);
  })
 }
 </script>
 
 <style scoped>
 </style>
 