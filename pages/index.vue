<template>
    <div class="min-h-screen flex items-center justify-center bg-gradient-to-t from-blue-900 to-blue-100">
      <div class="w-full max-w-md p-8 space-y-6 bg-white shadow-lg rounded-lg">
        <h2 class="text-3xl font-bold text-gray-900 text-center">SignIN</h2>
  
        <form class="mt-8 space-y-6" @submit.prevent="handleSubmit">
          <div class="rounded-md shadow-sm -space-y-px">
            <div>
              <div>Username</div>
              <input v-model="username" required class="form-control" placeholder="username">
            </div>
            <div class="pt-5">
              <div>Password</div>
              <input v-model="password" required class="form-control" placeholder="password">
            </div>
          </div>
  
          <div class="flex justify-between">
            <div class="flex">
              <input id="remember-me" type="checkbox" class="h-4 w-4 text-indigo-600 focus:ring-indigo-500 rounded">
              <label for="remember-me" class="ml-2 block text-sm text-gray-900 ">remember me</label>
            </div>
  
            <div class="text-sm">
              <a href="" class="font-medium text-indigo-600 hover:text-indigo-500">Forgot passwprd?</a>
            </div>
          </div>
  
          <div>
            <button type="submit" class="btn-full">Sing In</button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script setup>
    import axios from 'axios'
    import Swal from 'sweetalert2'
    import config from '@/config'
  
    const username = ref('')
    const password = ref('')
  
    const handleSubmit = async () => {
      try {
        if(!username.value || !password.value) {
          Swal.fire({
            icon: 'error',
            title: 'Error',
            text: 'username & password are required',
          })
        } else {
          const response = await axios.post(`${config.apiServer}/api/user/signIn`, {
            username: username.value,
            password: password.value,
          })
  
          if(response.status === 200) {
            console.log(response.data)
            localStorage.setItem(config.token, response.data.token);
            localStorage.setItem('nuxt_erp_user_id', response.data.id);

            navigateTo('/home');
          } else {
            Swal.fire({
              icon: 'error',
              title: 'Error',
              text:'Invalid username or password',
            })
          }
        }
      } catch(error) {
        Swal.fire({
          icon: 'error',
          title: 'Error',
          text: error.message,
        })
      }
    }
  </script>
  