<script setup lang="ts">
import { ref } from "vue";
import axios from "../utils/axios";

const email = ref("");
const password = ref("");
async function login() {
  try {
    const { data } = await axios.post("/login", {
      email: email.value,
      password: password.value,
    });

    if (data.message === "invalid_credentials") {
      alert("invalid credentials");
      return;
    }
    window.localStorage.setItem("token", data?.data?.token);
    alert("user logged!");
  } catch (error: any) {
    if (error?.response?.data?.message) {
      alert(error?.response?.data?.message);
      return;
    }
  }
}
</script>

<template>
  <div class="flex justify-center items-center h-screen">
    <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
      <div class="mb-4">
        <label class="block text-gray-700 text-sm font-bold mb-2" for="email">
          Email
        </label>
        <input
          v-model="email"
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          id="email"
          type="text"
          placeholder="Enter your email"
        />
      </div>
      <div class="mb-6">
        <label
          class="block text-gray-700 text-sm font-bold mb-2"
          for="password"
        >
          Password
        </label>
        <input
          v-model="password"
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          id="password"
          type="password"
          placeholder="Enter your password"
        />
      </div>
      <div class="flex items-center justify-between">
        <button
          @click.prevent="login"
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="button"
        >
          Sign In
        </button>
        <a
          class="inline-block align-baseline font-bold text-sm text-blue-500 hover:text-blue-800"
          href="#"
        >
          Forgot Password?
        </a>
      </div>
    </form>
  </div>
</template>