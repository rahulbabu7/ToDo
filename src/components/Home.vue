<template>
  <div class="flex flex-col min-h-screen bg-gray-700">
    <main class="flex-grow flex items-center justify-center">
      <section class="p-8 ">
        <span class="flex justify-center">
          <h2 class="text-base mb-4 text-white">Welcome to NotesApp</h2>
        </span>
        
        <h2 class="text-base text-white mb-4">Login with your account to continue</h2>
        <span class="ml-auto flex justify-center">
          <button class="bg-green-600 hover:bg-green-700 text-white font-semibold py-2 px-4 rounded">
          Log in
        </button>
        <button class="bg-green-600 hover:bg-green-700 text-white font-semibold py-2 px-4 rounded ml-4">
          Sign up
        </button>
      </span>
       
      </section>
    </main>
    <footer class="mt-auto mb-8 text-center flex justify-center gap-x-6">
      <button class="text-neutral-500">Terms of use</button> 
      <button class="text-neutral-500">Privacy Policy</button> 
    </footer>
  </div>
</template>
<script setup>

</script>
<style scoped>

</style>
