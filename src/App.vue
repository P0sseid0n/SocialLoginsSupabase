<script setup lang="ts">
import { ref } from 'vue'
import { supabase } from './supabase'
import type { User, Provider } from '@supabase/supabase-js'

const user = ref<User>()

supabase.auth.onAuthStateChange((event, session) => {
   user.value = session?.user ?? undefined
})

async function handleLogin(provider: Provider) {
   try {
      const { error } = await supabase.auth.signInWithOAuth({ provider })

      if (error) throw error

   } catch (error) {
      if (error instanceof Error) {
         alert(error.message)
      }
   }
}

function handleSignOut() {
   supabase.auth.signOut()
}

</script>

<template>
   <h1>Social Logins</h1>

   <main v-if="user">
      <img :src="user.user_metadata.avatar_url" alt="avatar" />

      <h2>{{ user.user_metadata.full_name }}</h2>
      <h3>{{ user.email }} </h3>
   </main>

   <div id="Buttons" v-if="user">
      <button @click="handleSignOut" class="signOutButton">SignOut</button>
   </div>
   <div id="Buttons" v-else>
      <button @click="() => handleLogin('twitter')">Twitter</button>
      <br>
      <button @click="() => handleLogin('github')">Github</button>
      <br>
      <button @click="() => handleLogin('discord')">Discord</button>
   </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap');

* {
   margin: 0;
   padding: 0;
   box-sizing: border-box;
   font-family: 'Inter', sans-serif;
   -webkit-font-smoothing: antialiased;
   -moz-osx-font-smoothing: grayscale;
}

#app {
   height: 100vh;

   background-color: #151922;
   color: white;

   display: flex;
   align-items: center;
   justify-content: center;
   flex-direction: column;

   text-align: center;


}

h1 {
   margin-bottom: 64px;
}

main {
   text-align: center;
   margin-top: 32px;
   margin-bottom: 64px;
}

main img {
   width: 128px;
   height: 128px;
   border-radius: 50%;
   margin-bottom: 16px;
}

main h2 {
   font-size: 1.5rem;
   font-weight: 500;
   margin-bottom: 8px;
}

main h3 {
   font-size: 1rem;
   font-weight: 400;
   margin-bottom: 16px;
   opacity: 0.8;
}



button {
   border: none;
   padding: 1rem 2rem;
   border-radius: 0.5rem;
   font-size: 1rem;
   font-weight: 500;
   cursor: pointer;

   margin-bottom: 16px;

   transition: opacity 0.2s ease-in-out;
}

button:hover {
   opacity: 0.8;
}

.signOutButton {
   background-color: #c21d1d;
   color: white;
}
</style>
