<template>
    <div>
      <h1 class="mb-3">Login Page</h1>
      <div class="flex flex-col gap-2">
        <input
          type="text"
          required
          v-model="username"
          class="border rounded-sm mb-2"
          placeholder=" Username"
        />
        <input
          type="password"
          required
          v-model="password"
          class="border rounded-sm mb-2"
          placeholder=" Password"
        />
        <button
          @click="onLogin()"
          :disabled="!isFormValid"
          class="rounded-md bg-green-500 text-white py-1 px-3 hover:bg-green-700"
        >
          Login
        </button>
      </div>
    </div>
</template>
  
<script setup lang="ts">
  import { ref } from "vue";
  import { useRouter } from "vue-router";
  import { useAuthStore } from "@/stores/auth";
  import axios from "axios";
  
  const auth = useAuthStore();
  const username = ref("");
  const password = ref("");
  const router = useRouter();
  
  // Check if both username and password are not empty
  const isFormValid = () => {
    return username.value.trim() !== "" && password.value.trim() !== "";
  };
  
  const onLogin = async () => {
    try {
      const respond =  await axios.post('http://localhost:3000/login', {
          username: username.value,
          password: password.value
      });

      console.log(respond)
      
      if (respond.data.status === "success"){
        auth.login(username.value);
        router.push("/");
      } else {
        window.alert("Username atau password salah!")
      }
    } catch (error){
      console.error("Error during login:", error);
      window.alert("Terjadi kesalahan saat melakukan login");
    }
  };
</script>