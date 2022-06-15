<template>
  <div class="flex justify-center items-center min-h-screen bg-white">
  <div class="block p-12 px-20 rounded-lg shadow-lg bg-gray-100 w-2/6">

    <form @submit.prevent="matchOtp">
      <div class="form-group mb-6">
        <h2 class="text-center font-bold text-xl">Percobaan OTP</h2>
      </div>

      <div class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 flex-col rounded mb-5" role="alert" v-if="isError">
        <p><strong class="font-bold">Error!</strong></p>
        <p class="block sm:inline">{{ errors }}</p>
      </div>

      <div class="form-group mb-6 flex">
        <input type="text" class="form-control block
          w-1/4
          px-5
          py-3
          text-base
          font-normal
          text-gray-700
          bg-white bg-clip-padding
          border border-solid border-gray-300
          rounded
          transition
          ease-in-out
          m-0
          mx-4
          focus:text-gray-700 focus:bg-white focus:border-purple-600 focus:outline-none"
          placeholder="x" 
          v-model="code1"
          maxlength="1"
          required
          />
        <input type="text" class="form-control block
          w-1/4
          px-5
          py-3
          text-base
          font-normal
          text-gray-700
          bg-white bg-clip-padding
          border border-solid border-gray-300
          rounded
          transition
          ease-in-out
          m-0
          mx-4
          focus:text-gray-700 focus:bg-white focus:border-purple-600 focus:outline-none"
          placeholder="x" 
          v-model="code2"
          maxlength="1"
          required
          />
        <input type="text" class="form-control block
          w-1/4
          px-5
          py-3
          text-base
          font-normal
          text-gray-700
          bg-white bg-clip-padding
          border border-solid border-gray-300
          rounded
          transition
          ease-in-out
          m-0
          mx-4
          focus:text-gray-700 focus:bg-white focus:border-purple-600 focus:outline-none"
          placeholder="x" 
          v-model="code3"
          maxlength="1"
          required
          />
        <input type="text" class="form-control block
          w-1/4
          px-5
          py-3
          text-base
          font-normal
          text-gray-700
          bg-white bg-clip-padding
          border border-solid border-gray-300
          rounded
          transition
          ease-in-out
          m-0
          mx-4
          focus:text-gray-700 focus:bg-white focus:border-purple-600 focus:outline-none"
          placeholder="x" 
          v-model="code4"
          maxlength="1"
          required
          />
      </div>

      <button type="submit" class="
        w-full
        mb-6
        px-5
        py-4
        bg-purple-600
        text-white
        font-medium
        text-xs
        leading-tight
        uppercase
        rounded
        shadow-md
        hover:bg-purple-700 hover:shadow-lg
        focus:bg-purple-700 focus:shadow-lg focus:outline-none focus:ring-0
        active:bg-purple-800 active:shadow-lg
        transition
        duration-150
        ease-in-out"
        @click="matchOtp">Verifikasi</button>

    </form>

    <p class="
      w-full
      text-center
      text-black
      hover:underline
      active:underline
      font-medium
      text-xs
      cursor-pointer
      transition
      duration-150
      ease-in-out"
      @click="requestOtp">Kirim ulang kode OTP</p>
      
  </div>
</div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      code1: undefined,
      code2: undefined,
      code3: undefined,
      code4: undefined,
      otp: [],
      userData: undefined,
      errors: undefined,
      isError: false
    }
  },
  methods: {
    getItem (KEY, defaultValue = null) {
      const value = localStorage.getItem(KEY) ? JSON.parse(localStorage.getItem(KEY)) : null
      if (!value) {
        return defaultValue
      }
        return value
    },

    requestOtp() {
      const userData = this.getItem('pretest_user');
      axios.post('/api/v1/register/otp/request', {
        phone: userData.phone
      })
      .then(response => {
        const data = response.data.data;
      })
      .catch(err => {
        let data = err.response.data;
        this.errors = data.error.errors[0];
        this.isError = true;
        setTimeout(() => {
          this.isError = false;
        }, 5000);
      })
    },

    matchOtp() {
      const userData = this.getItem('pretest_user');
      axios.post('/api/v1/register/otp/match', {
        user_id: userData.id,
        otp_code: `${this.code1}${this.code2}${this.code3}${this.code4}`
      })
      .then(response => {
        const data = response.data.data;
        localStorage.setItem('access_token', JSON.stringify(data.user.access_token));
        this.$router.push({ path: `/profile` });
      })
      .catch(err => {
        let data = err.response.data;
        this.errors = data.error.errors[0];
        this.isError = true;
        setTimeout(() => {
          this.isError = false;
        }, 5000);
      })
    },
  }
}
</script>
