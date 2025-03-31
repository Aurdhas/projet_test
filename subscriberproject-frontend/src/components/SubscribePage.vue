<template>
    <div class="flex items-center justify-center min-h-screen bg-gray-900 p-6">
        <div v-if="!subscribed" class="bg-white rounded-2xl shadow-lg flex flex-col  md:flex-row overflow-hidden w-full max-w-4xl p-6">
            <div class="p-8 md:w-1/2 flex flex-col justify-center space-y-6">
                <h1 class="text-5xl font-bold text-gray-900">
                    Stay updated!
                </h1>
                <p class="text-gray-600 mt-2">Join 60,000+ product managers receiving monthly updates on: </p>
                <div>
                    <ul class="mt-4 space-y-2">
                  <li class="flex items-center text-gray-700">
                    <img src="/home/aurdhas/projet_test/subscriberproject-frontend/src/assets/images/icon-list.svg" alt="Check" class="w-4 h-4 mr-2"> Product discovery and building what matters
                  </li>
                  <li class="flex items-center text-gray-700">
                    <img src="/home/aurdhas/projet_test/subscriberproject-frontend/src/assets/images/icon-list.svg" alt="Check" class="w-4 h-4 mr-2"> Measuring to ensure updates are a success
                  </li>
                  <li class="flex items-center text-gray-700">
                    <img src="/home/aurdhas/projet_test/subscriberproject-frontend/src/assets/images/icon-list.svg" alt="Check" class="w-4 h-4 mr-2"> And much more!
                  </li>
                </ul>
                </div>
                <div class="mt-6">
                  <div class="flex justify-between">
                    <label class="block text-sm font-semibold text-gray-700">Email address</label>
                    <span v-if="error" class="text-red-500 text-xs">{{ error }}</span>
                  </div>
                  <input 
                    v-model="email"
                    type="email" 
                    placeholder="email@company.com"
                    :class="{
                        'w-full mt-2 px-4 py-2 border rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500': true,
                        'border-red-500': error
                    }" 
                  />
                </div>
                <button 
                  @click="subscribe"
                  class="mt-4 w-full cursor-pointer bg-blue-900 text-white py-2 rounded-md font-semibold hover:bg-gradient-to-r from-red-500 to-pink-500">
                  Subscribe to monthly newsletter
                </button>
            </div>
            <div class="hidden md:block md:w-1/2 bg-cover bg-center relative">
                <img src="/home/aurdhas/projet_test/subscriberproject-frontend/src/assets/images/illustration-sign-up-desktop.svg" alt="Illustration" class="w-full h-full object-cover">
            </div>
        </div>
        <div v-if="subscribed" class="bg-white rounded-lg shadow-lg p-10 text-center max-w-sm">
            <img src="/home/aurdhas/projet_test/subscriberproject-frontend/src/assets/images/icon-success.svg" class="w-12 mx-auto mb-4" />
            <h2 class="text-2xl font-bold text-gray-900">Thanks for subscribing!</h2>
            <p class="text-gray-600 mt-2">
                A confirmation email has been sent to <strong>{{ email }}</strong>. 
                Please open it and click the button inside to confirm your subscription.
            </p>
            <button 
                @click="subscribed = false" 
                class="w-full mt-6 bg-blue-900 cursor-pointer text-white py-2 rounded-lg hover:hover:bg-gradient-to-r from-red-500 to-pink-500 transition">
                Dismiss message
            </button>
        </div>
    </div>
</template>
<script setup>
import { ref } from 'vue';
import axios from 'axios';

const email = ref('');
const error = ref('');
const subscribed = ref(false);

const subscribe = async () => {
  // Reset error
  error.value = '';
  
  // Simple email validation
  if (!email.value) {
    error.value = 'Email is required';
    return;
  }
  
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.value)) {
    error.value = 'Valid email required';
    return;
  }

  try {
    const response = await axios.post(
      'http://127.0.0.1:8000/api/subscribe',
      { email: email.value },
      {
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json'
        }
      }
    );
    subscribed.value = true;
  } catch (err) {
    console.error(err.response);
    if (err.response && err.response.data) {
      error.value = err.response.data.message || 'An unexpected error occurred.';
    } else {
      error.value = 'Server error. Please try again later.';
    }
  }
};
</script>
