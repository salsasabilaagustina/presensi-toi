<template>
    <div class="card rounded-5" style="width: 37rem; height: 45vh; background-color: #D9D9D9;">
  <div class="card-body">
    <h5 class="card-title text-center">LOGIN DULU YUK!</h5>
        <form action="">
            <input v-model="email" type="text" class="rounded-5 mb-5 my-5 ps-3" placeholder="Username" style="width: 35rem; height:3rem;background-color: #FFFFFF;">
            <input v-model="password" type="password" class="rounded-5 mb-5 ps-3" placeholder="Password" style="width: 35rem; height:3rem;background-color: #FFFFFF; ">
            <div class="row justify-content-center">
                <button  @click="handleLogin" type="button" class="btn rounded-5" style="width:8rem; background-color: #FFFFFF;">Login</button>
            </div>
            
        </form>
  </div>
</div>
</template>

<script setup>
definePageMeta({
  layout: 'loginpage',
  
})
const client = useSupabaseClient()
const email = ref("");
const password = ref("");

async function handleLogin() {
  console.log(email.value)
  console.log(password.value)
  const { data, error } = await client.auth.signInWithPassword({
    email: email.value,
    password: password.value
  })
  if (data) navigateTo('/presensi')
}
</script>