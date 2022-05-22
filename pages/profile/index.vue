<template>
  <section class="flex-col px-24 py-24">
    <div class="bg-cover bg-center bg-gray-100 text-white object-fill rounded flex justify-center items-center h-96"
    :style="{ 'background-image': `${getImgUrlCover()}` }"
    >
      <input
        ref="fileUpload"
        type="file"
        id="cover"
        @input="pickFileCover"
        class="text-right text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-purple-50 file:text-purple-700 hover:file:bg-purple-100 cursor-pointer"
      >

      <button type="submit" class="
        w-20
        p-2
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
        @click="uploadCover"
        v-if="coverPreviewImage">Save</button>
        
    </div>

    <div class="w-full flex py-12">
      <div class="w-1/5 mr-12">
        <div class="h-64 mb-5 bg-cover bg-center object-fill shadow-sm bg-gray-100 rounded-full flex justify-center items-center" :style="{ 'background-image': `${getImgUrlUser()}` }">

        </div>
        <div class="w-full min-h-96 h-auto max-h-auto mb-5 p-4 rounded shadow-sm bg-gray-100">
          <p class="text-left pt-1 text-gray-600">
            <span class="flex justify-end items-center cursor-pointer">
              <img class="h-full w-4 mr-2" src="../../assets/pencil.svg" alt="edit" @click="editContent('Profile')" v-if="!isProfile"/>
              Profile
            </span>
          </p>
          <div class="h-84 overflow-auto">
            <div class="h-52 overflow-auto" v-if="!isProfile">
              <h4  class="text-gray-500 text-sm py-2">Nama : {{ userData && userData.name }}</h4>
              <h4 class="text-gray-500 text-sm py-2">Gender : {{ userData && userData.gender === 'male' ? 'Laki-laki' : 'Perempuan' }}</h4>
              <h4 class="text-gray-500 text-sm py-2">Lahir : {{ userData && userData.birthday }}</h4>
              <h4 class="text-gray-500 text-sm py-2">Kota Asal : {{ userData && userData.hometown }}</h4>
              <h4 class="text-gray-500 text-sm py-2">Bio : {{ userData && userData.bio }}</h4>
            </div>

            <form class="w-full" @submit.prevent="updateProfile" v-if="isProfile">
              <div class="flex items-center border-b border-purple-500 py-2">
                <span class="flex-shrink-0 border-transparent border-4 text-purple-500 text-sm py-1 px-2 rounded">
                  Nama
                </span>
                <input class="appearance-none bg-transparent border-none w-full text-gray-700 text-sm mr-3 py-1 px-2 leading-tight focus:outline-none" type="text" placeholder="Name" aria-label="Name" v-model="name">
              </div>
              <div class="flex items-center border-b border-purple-500 py-2">
                <span class="flex-shrink-0 border-transparent border-4 text-purple-500 text-sm py-1 px-2 rounded">
                  Gender
                </span>
                <select class="form-select appearance-none
                  block
                  w-1/2
                  px-3
                  py-1.5
                  text-sm
                  font-normal
                  text-gray-700
                  bg-gray-100 bg-clip-padding bg-no-repeat
                  border border-gray-50
                  rounded
                  transition
                  ease-in-out
                  m-0
                  focus:text-gray-700 focus:outline-none" aria-label="Default select example" v-model="gender">
                    <option value="undefined" selected>- Gender -</option>
                    <option value="0">Male</option>
                    <option value="1">Female</option>
                </select>
              </div>
              <div class="flex items-center border-b border-purple-500 py-2">
                <span class="flex-shrink-0 border-transparent border-4 text-purple-500 text-sm py-1 px-2 rounded">
                  Lahir
                </span>
                <input type="date" id="birthday" name="birthday" class="bg-gray-100 outline-none text-gray-500 text-sm" v-model="birthday">
              </div>

              <div class="flex items-center border-b border-purple-500 py-2">
                <span class="flex-shrink-0 border-transparent border-4 text-purple-500 text-sm py-1 px-2 rounded">
                  Kota Asal
                </span>
                <input class="appearance-none bg-transparent border-none w-full text-gray-700 text-sm mr-3 py-1 px-2 leading-tight focus:outline-none" type="text" placeholder="Hometown" aria-label="Hometown" v-model="hometown">
              </div>

              <div class="flex-col items-center border-b border-purple-500 py-2">
                <span class="flex-shrink-0 border-transparent border-4 text-purple-500 text-sm py-1 px-2 rounded">
                  Bio
                </span>
                <textarea
                  class="appearance-none bg-transparent border-none w-full text-gray-700 text-sm py-1 px-2 leading-tight focus:outline-none"
                  rows="3"
                  placeholder="Your message"
                  v-model="bio"
                ></textarea>
              </div>

              <div class="flex flex-row-reverse">
                <button type="submit" class="
                  w-full
                  p-2 mt-2
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
                  >Save</button>
              </div>
            </form>

            <div class="flex flex-row-reverse">
              <button type="submit" class="
                w-full
                p-2 mt-2
                bg-red-600
                text-white
                font-medium
                text-xs
                leading-tight
                uppercase
                rounded
                shadow-md
                hover:bg-red-700 hover:shadow-lg
                focus:bg-red-700 focus:shadow-lg focus:outline-none focus:ring-0
                active:bg-red-800 active:shadow-lg
                transition
                duration-150
                ease-in-out"
                @click="logoutUser">Logout</button>
              </div>
          </div>
        </div>

        <div class="w-full min-h-96 h-auto max-h-auto mb-5 p-4 rounded shadow-sm bg-gray-100">
          <p>Message</p>

          <div class="w-full flex-col mt-5" v-if="chatList.length">
            <div class="w-full bg-white-100" v-for="item in chatList" :key="item.id">
              <p class="font-bold italic text-gray-500">From : {{ item.user_sender.name }}</p>
              <p>"{{ item.message }}"</p>
            </div>
          </div>

        </div>
      </div>

      <div class="w-4/5 flex-col">
        <div class="flex w-full mr-12">
          <div class="w-1/2 mr-5 h-66 rounded shadow-sm bg-gray-100 mb-5 p-6 flex-col">
            <div class="h-52 overflow-auto" v-if="!isCareer">
              <h4  class="text-gray-500 py-3 text-center">Nama Perusahaan : {{ userData && userData.career && userData.career.company_name }}</h4>
              <h4 class="text-gray-500 py-3 text-center">Mulai bekerja pada : {{ userData && userData.career && userData.career.starting_from }}</h4>
              <h4 class="text-gray-500 py-3 text-center">Selesai bekerja pada : {{ userData && userData.career && userData.career.ending_in }}</h4>
              
            </div>
            
            <div class="h-52 overflow-auto" v-if="isCareer">
              <form class="w-full" @submit.prevent="updateCareer">
                <div class="flex items-center border-b border-purple-500 py-2">
                  <span class="flex-shrink-0 border-transparent border-4 text-purple-500 text-sm py-1 px-2 rounded">
                    Nama Perusahaan
                  </span>
                  <input class="
                    appearance-none bg-transparent border-none 
                    w-full 
                    text-gray-700 text-sm 
                    mr-3 py-1 px-2 
                    leading-tight focus:outline-none" 
                    type="text" 
                    placeholder="Nama Perusahaan" 
                    aria-label="Full Company name" 
                    v-model="career.company_name"
                  >
                </div>
                <div class="flex items-center border-b border-purple-500 py-2">
                  <span class="flex-shrink-0 border-transparent border-4 text-purple-500 text-sm py-1 px-2 rounded">
                    Mulai bekerja pada
                  </span>
                  <input type="date" id="starting_from" name="starting_from" class="bg-gray-100 outline-none text-gray-500 text-sm" v-model="career.starting_from">
                </div>
                <div class="flex items-center border-b border-purple-500 py-2">
                  <span class="flex-shrink-0 border-transparent border-4 text-purple-500 text-sm py-1 px-2 rounded">
                    Selesai bekerja pada
                  </span>
                  <input type="date" id="ending_in" name="ending_in" class="bg-gray-100 outline-none text-gray-500 text-sm" v-model="career.ending_in">
                </div>

                <div class="flex flex-row-reverse">
                  <button type="submit" class="
                    w-20
                    p-2 mt-2
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
                    >Save</button>
                </div>

              </form>
            </div>
            <p class="text-right pt-1 text-gray-600">
              <span class="flex justify-end items-center  cursor-pointer">
                <img class="h-full w-4 mr-2" src="../../assets/pencil.svg" alt="edit" @click="editContent('Career')" v-if="!isCareer"/>
                Riwayat Karir
              </span>
            </p>
          </div>
          <div class="w-1/2 h-66 rounded shadow-sm bg-gray-100 mb-5 p-6 flex-col">
            <div class="h-52 overflow-auto" v-if="!isEducation">
              <h4 class="text-gray-500 py-3 text-center">Nama Sekolah : {{ userData && userData.education && userData.education.school_name }}</h4>
              <h4 class="text-gray-500 py-3 text-center">Lulus pada : {{ userData && userData.education && userData.education.graduation_time }}</h4>
            </div>

            <div class="h-52 overflow-auto" v-if="isEducation">
              <form class="w-full" @submit.prevent="updateEducation">
                <div class="flex items-center border-b border-purple-500 py-2">
                  <span class="flex-shrink-0 border-transparent border-4 text-purple-500 text-sm py-1 px-2 rounded">
                    Nama Sekolah
                  </span>
                  <input class="appearance-none bg-transparent border-none w-full text-gray-700 text-sm mr-3 py-1 px-2 leading-tight focus:outline-none" type="text" placeholder="Nama Sekolah" aria-label="Full School name" v-model="education.school_name">
                </div>
                <div class="flex items-center border-b border-purple-500 py-2">
                  <span class="flex-shrink-0 border-transparent border-4 text-purple-500 text-sm py-1 px-2 rounded">
                    Lulus pada
                  </span>
                  <input type="date" id="starting_from" name="starting_from" class="bg-gray-100 outline-none text-gray-500 text-sm" v-model="education.graduation_time">
                </div>

                <div class="flex flex-row-reverse">
                  <button type="submit" class="
                    w-20
                    p-2 mt-2
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
                    >Save</button>
                </div>
              </form>
            </div>
            <p class="text-right pt-1 text-gray-600">
              <span class="flex justify-end items-center cursor-pointer">
                <img class="h-full w-4 mr-2" src="../../assets/pencil.svg" alt="edit" @click="editContent('Education')" v-if="!isCareer"/>
                Riwayat Pendidikan
              </span>
            </p>
          </div>
        </div>

        <div class="flex w-full">
          <div class="w-full h-auto rounded shadow-sm bg-gray-100 mb-5 p-6 flex-col">
            <p class="text-right pt-1 text-gray-600">
              <span class="flex justify-end items-center cursor-pointer">
                <img class="h-full w-4 mr-2" src="../../assets/pencil.svg" alt="edit" @click="editContent('Education')" v-if="!isCareer"/>
                Gallery
              </span>
            </p>
            <div class="h-auto w-full overflow-auto flex flex-wrap m-6" v-if="userData && userData.user_pictures">
              <div class="w-1/5 m-4 rounded-lg shadow-lg bg-white max-w-sm" v-for="item in userData.user_pictures" :key="item.id">
                <div class="bg-cover bg-center bg-gray-100 text-white object-fill rounded flex justify-center items-center h-48"
                :style="{ 'background-image': `url(${item.picture && item.picture.url})` }"></div>
                <div class="flex justify-end p-2">
                  <span class="cursor-pointer">
                    <img class="h-full w-4 mr-2" src="../../assets/trash.svg" alt="edit" @click="deleteGallery(item.id)"/>
                  </span>
                  <span class="cursor-pointer">
                    <img class="h-full w-4 mr-2" src="../../assets/user.svg" alt="default" @click="uploadProfile(item.id)"/>
                  </span>
                </div>
              </div>
              <div class="w-2/5 m-4 rounded-lg shadow-lg bg-white max-w-sm">
                <div class="bg-cover bg-center bg-gray-100 text-white object-fill rounded flex justify-center items-center h-48">
                  <input
                    ref="fileInput"
                    type="file"
                    id="user"
                    @input="pickFileUser"
                    class="text-center text-sm p-2 text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-purple-50 file:text-purple-700 hover:file:bg-purple-100 cursor-pointer"
                    multiple
                  >

                  <button type="submit" class="
                    w-20
                    p-2
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
                    @click="uploadGallery"
                    >Save</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
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
      career: {
        company_name: undefined,
        starting_from: undefined,
        ending_in: undefined,
        position: "Staff"
      },
      education: {
        school_name: undefined,
        graduation_time: undefined,
      },
      name: undefined,
      gender: undefined,
      birthday: undefined,
      hometown: undefined,
      bio: undefined,
      errors: undefined,
      isError: false,
      coverPreviewImage: undefined,
      userPreviewImage: undefined,
      coverImage: undefined,
      userImage: [],
      fileImage: undefined,
      access_token: undefined,
      user_pictures: [],
      isCareer: false,
      isEducation: false,
      isProfile: false,
      chatList: []
    }
  },

  mounted() {
    this.access_token = localStorage.getItem('access_token') ? JSON.parse(localStorage.getItem('access_token')) : null;

    axios.get('/api/v1/profile/me', {
      headers: {
        Authorization: `${this.access_token}`,
      },
    })
    .then(response => {
      const data = response.data.data;
      this.userData = data.user;
      localStorage.setItem('pretest_user', JSON.stringify(data.user));
      this.getMessage(data.user.id);
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

  methods: {
    getMessage(id) {
      axios.get(`/api/v1/message/${'3c38afef-6b6d-4b41-af11-86d35c8392ce'}`, {
        headers: {
          Authorization: `${this.access_token}`,
        },
      })
      .then(response => {
        const data = response.data.data;
        this.chatList = data.chat;
        console.log(data.chat)
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

    getImgUrlCover() {
      return this[`coverPreviewImage`] ? `url(${this[`coverPreviewImage`]})` : this.userData && this.userData[`cover_picture`] && this.userData[`cover_picture`].url ? `url(${this.userData[`cover_picture`].url})` : 'none';
    },

    getImgUrlUser() {
      return this[`userPreviewImage`] ? `url(${this[`userPreviewImage`]})` : this.userData && this.userData[`user_picture`] && this.userData[`user_picture`].picture && this.userData[`user_picture`].picture.url ? `url(${this.userData[`user_picture`].picture.url})` : 'none';
    },

    pickFileCover() {
      let input = this.$refs.fileUpload;
      let file = input.files;

      if (file && file[0]) {
        let reader = new FileReader;
        reader.onload = e => {
          this.coverPreviewImage  = e.target.result;
        }
        reader.readAsDataURL(file[0]);
        this.$emit('input', file[0]);

        this.coverImage = file[0];
      }
    },

    editContent(target) {
      this[`is${target}`] = true;
    },

    pickFileUser() {
      let input = this.$refs.fileInput;
      let file = input.files;

      if (file && file.length) {
        for(let i; i < file.length; i++) {
          this.userImage.push(file[i]);
          let reader = new FileReader;
          reader.onload = e => {
            this.userPreviewImage  = e.target.result;
          }
          reader.readAsDataURL(file[i]);
          this.$emit('input', file[i]);
        }
      }

    },

    uploadCover() {
      let data = new FormData();
      data.append('image', this.coverImage ); 

      axios.post(
        `/api/v1/uploads/cover`, data, {
          headers: {
            Authorization: `${this.access_token}`,
          },
        }
      ).then(
        response => {
          if(response) {
            this.coverPreviewImage = undefined;
          }
        }
      ).catch(err => {
        let data = err.response.data;
        this.errors = data.error.errors[0];
        this.isError = true;
        setTimeout(() => {
          this.isError = false;
        }, 5000);
      }).finally(() => {
        location.reload();
      })
    },

    uploadProfile(_id) {
      axios.post(
        `/api/v1/uploads/profile/default`, {
          id: _id
        }, {
          headers: {
            Authorization: `${this.access_token}`,
          },
        }
      )
      .then(
        response => {
          const data = response.data.data;
        }
      )
      .catch(err => {
        let data = err.response.data;
        this.errors = data.error.errors[0];
        this.isError = true;
        setTimeout(() => {
          this.isError = false;
        }, 5000);
      })
      .finally(() => {
        location.reload();
      })
    },

    uploadGallery() {
      let data = new FormData();
      for( let i = 0; i < this.$refs.fileInput.files.length; i++ ){
        let file = this.$refs.fileInput.files[i];
        data.append(`image`, file);
      }

      axios.post(
        `/api/v1/uploads/profile`, data, {
          headers: {
            Authorization: `${this.access_token}`,
          },
        }
      )
      .then(
        response => {
          const data = response.data.data;
          location.reload();
        }
      )
      .catch(err => {
        let data = err.response.data;
        this.errors = data.error.errors[0];
        this.isError = true;
        setTimeout(() => {
          this.isError = false;
        }, 5000);
      })
    },

    deleteGallery(_id) {
      axios.delete('/api/v1/uploads/profile', {
        id: _id
      }, {
        headers: {
          Authorization: `${this.access_token}`,
        },
      })
      .then(response => {
        const data = response.data.data;
        location.reload();
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

    logoutUser() {
      axios.post('/api/v1/oauth/revoke', {
        confirm: '1'
      }, {
        headers: {
          Authorization: `${this.access_token}`,
        },
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
      .finally(() => {
        this.$router.push({ path: `/` });
        localStorage.removeItem('access_token');
        localStorage.removeItem('pretest_user');
      })
    },

    updateCareer() {
      axios.post('/api/v1/profile/career', this.career, {
        headers: {
          Authorization: `${this.access_token}`,
        },
      })
      .then(response => {
        const data = response.data.data;
        this.isCareer = false;
        location.reload();
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

    updateEducation() {
      axios.post('/api/v1/profile/education', this.education, {
        headers: {
          Authorization: `${this.access_token}`,
        },
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

    updateProfile() {
      axios.post('/api/v1/profile', {
        name: this.name,
        gender: this.gender,
        birthday: this.birthday,
        hometown: this.hometown,
        bio: this.bio,
      }, {
        headers: {
          Authorization: `${this.access_token}`,
        },
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
  }
}
</script>
