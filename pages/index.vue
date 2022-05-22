<template>
  <div class="flex justify-center items-center min-h-screen bg-white">
    <div class="block p-12 px-20 rounded-lg shadow-lg bg-gray-100 w-2/6">
      <form @submit.prevent="onSubmit">
        <div class="form-group mb-6">
          <h2 class="text-center font-bold text-xl">Registration</h2>
        </div>

        <div class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 flex-col rounded" role="alert" v-if="isError">
          <p><strong class="font-bold">Error!</strong></p>
          <p class="block sm:inline">{{ errors }}</p>
        </div>

        <div class="form-group mb-6 mt-6">
          <input type="text" class="form-control
            block
            w-full
            px-3
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
            focus:text-gray-700 focus:bg-white focus:border-purple-600 focus:outline-none"
            aria-describedby="phone" placeholder="Phone" 
            v-model="phone"
            required
            />
        </div>

        <div class="form-group mb-6">
          <input type="password" class="form-control block
            w-full
            px-3
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
            focus:text-gray-700 focus:bg-white focus:border-purple-600 focus:outline-none"
            placeholder="Password" 
            v-model="password"
            required
            />
        </div>

        <div class="form-group mb-6">
          <input type="text" class="form-control block
            w-full
            px-3
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
            focus:text-gray-700 focus:bg-white focus:border-purple-600 focus:outline-none"
            placeholder="Country" 
            v-model="country"
            required
            />
        </div>
        
        <button type="submit" class="
          w-full
          px-6
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
          ease-in-out">Register</button>
      </form>

      <nuxt-link to="/login">
        <p class="text-center uppercase font-bold text-sm pt-4 text-purple-500 hover:text-purple-800">
          Login Here
        </p>
      </nuxt-link>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      phone: undefined,
      password: undefined,
      country: undefined,
      latlong: 'latlong',
      device_token: 'device_token',
      device_type: 2,
      errors: undefined,
      isError: false,
    }
  },
  methods: {
    onSubmit () { 
      axios.post('/api/v1/register', {
        phone: this.phone,
        password: this.password,
        country: this.country,
        latlong: 'latlong',
        device_token: 'device_token',
        device_type: 2
      })
      .then(response => {
        const data = response.data.data;
        localStorage.setItem('pretest_user', JSON.stringify(data.user));
        this.$router.push({ path: `/otp` });
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