<template>
  <div>
    <main ref="main" class="main" :class="[vuexMenuState ? 'blur' : '',vuexLogregMobileState ? 'blur' : '']" @click="closeMenu">
      <div class="user-profile-title">
        <div class="mobile-icon">
          <a href="javascript:void(0)" @click="openPageMenu">
            <img src="/images/burger.svg" alt="openPageMenu">
          </a>
        </div>
        <div class="inner-title">
          <div>
            <div class="img">
              <img :src="$auth.user.image" :alt="$auth.user.name">
            </div>
            <div class="name-mobile">
              <p class="name">{{ $auth.user.name }}</p>
              <span class="mobile">{{ $auth.user.mobile }}</span>
              <p class="deposit">
                <span>موجودی : </span>
                <span>{{ number_3_3($auth.user.wallet) }} تومان</span>
              </p>
            </div>
          </div>
        </div>
      </div>

      <section class="set-place">
        <div class="body reserve-list finance">
          <h3 class="page-name">
            <i class="bx bx-building-house"></i>
            مدیریت اقامتگاه
          </h3>
          <div class="sidebar-form">
            <div class="sidebar">
              <ul v-show="menu === true">
                <li>
                  <nuxt-link to="/profile">
                    <span class="counter"><i class="bx bx-home"></i></span>
                    <span class="name">پروفایل</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/`+id">
                    <span class="counter">1</span>
                    <span class="name">اطلاعات اصلی</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/detail/`+id">
                    <span class="counter">2</span>
                    <span class="name">اطلاعات جزئی</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/calendar/`+id">
                    <span class="counter">3</span>
                    <span class="name">تقویم کاری</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/images/`+id">
                    <span class="counter">4</span>
                    <span class="name">تصاویر</span>
                  </nuxt-link>
                </li>
                <li class="active">
                  <nuxt-link :to="`/profile/residence/edit/roadOptions/`+id">
                    <span class="counter">5</span>
                    <span class="name">اطلاعات محیطی</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/facilities/`+id">
                    <span class="counter">6</span>
                    <span class="name">امکانات اقامتگاه</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/wcs/`+id">
                    <span class="counter">7</span>
                    <span class="name">سرویس بهداشتی و حمام</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/rooms/`+id">
                    <span class="counter">8</span>
                    <span class="name">اتاق ها</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/ownership/`+id">
                    <span class="counter">9</span>
                    <span class="name">اطلاعات مالکیت</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/personal/`+id">
                    <span class="counter">10</span>
                    <span class="name">اطلاعات فردی</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/parking/`+id">
                    <span class="counter">11</span>
                    <span class="name">پارکینگ</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/map/`+id">
                    <span class="counter">12</span>
                    <span class="name">نقشه</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/rules/`+id">
                    <span class="counter">13</span>
                    <span class="name">قوانین و مقررات</span>
                  </nuxt-link>
                </li>
                <li>
                  <nuxt-link :to="`/profile/residence/edit/crules/`+id">
                    <span class="counter">14</span>
                    <span class="name">قوانین کنسلی</span>
                  </nuxt-link>
                </li>
              </ul>
            </div>
            <div class="form">
              <p class="form-name w-100 uk-margin-bottom">اطلاعات محیطی</p>
              <p class="form-description">
                اطلاعات محیطی اقامتگاه مانند مسیرهای دسترسی و امکانات محیطی اقامتگاه را وارد فرمایید.
              </p>
              <form action="">
                <div class="row">
                  <p class="form-name w-100 uk-margin-medium-bottom">مسیرهای دسترسی</p>
                  <div class="form-group checkbox w-30" v-for="roadOption in roadOptions">
                    <input type="checkbox" class="uk-checkbox" :id="`roadOption`+roadOption.id"
                           :checked="roadOption.checked" @change="changeRoadOption(roadOption)">
                    <label :for="`roadOption`+roadOption.id">{{ roadOption.title }}</label>
                  </div>
                </div>
                <hr>
                <div class="row">
                  <p class="form-name w-100 uk-margin-medium-bottom">امکانات محیطی اقامتگاه</p>
                  <div class="form-group checkbox w-30" v-for="environmentalOption in environmentalOptions">
                    <input type="checkbox" class="uk-checkbox" :id="`environmentalOption`+environmentalOption.id"
                           :checked="environmentalOption.checked" @change="changeEnvironmentalOption(environmentalOption)">
                    <label :for="`environmentalOption`+environmentalOption.id">{{ environmentalOption.title }}</label>
                  </div>
                </div>
                <hr>
                <div class="row">
                  <p class="form-name w-100 uk-margin-medium-bottom">ثبت مکان جدید</p>
                  <p class="form-description">
                    مکان‌های ضروری اطراف اقامتگاه مانند سوپرمارکت، نانوایی، بیمارستان یا مکان‌های تفریحی، تاریخی و زیارتی اطراف اقامتگاه را اضافه کنید. توجه داشته باشید که وارد کردن این اطلاعات اختیاری است اما تاثیر مناسبی در جذب مسافر خواهد داشت.
                  </p>
                  <div class="form-group w-30">
                    <input type="text" id="title" v-model="nearByPlace.title" required>
                    <span class="floating-label">نام مکان مورد نظر</span>
                  </div>
                  <div class="form-group w-30">
                    <input type="text" id="distanceWalk" v-model="nearByPlace.distanceWalk" required>
                    <span class="floating-label">فاصله زمانی پیاده (دقیقه)</span>
                  </div>
                  <div class="form-group w-30">
                    <input type="text" id="distanceCar" v-model="nearByPlace.distanceCar" required>
                    <span class="floating-label">فاصله زمانی با خودرو (دقیقه)</span>
                  </div>
                </div>
                <div class="row submit-cancel">
                  <button type="button" class="submit" v-if="creating === false" @click="createNearbyPlace">ثبت مکان جدید</button>
                  <button type="button" class="submit" v-else>چند لحظه صبر کنید</button>
                </div>
              </form>
              <div class="show-lists">
                <hr>
                <p class="form-name w-100 uk-margin-medium-bottom">نزدیک ترین اماکن</p>
                <div class="uk-overflow-auto">
                  <table class="uk-table uk-table-middle ">
                    <thead>
                    <tr>
                      <th>#</th>
                      <th>عنوان</th>
                      <th>عملیات</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(nearByPlace,index) in nearByPlaces">
                      <td class="date" style="padding-right: 15px;">{{ index+1 }}</td>
                      <td class="date">{{ nearByPlace.title }}</td>
                      <td class="more-action">
                        <a href="javascript:void(0)" class="editBtn" @click="setNearbyPlace(nearByPlace)">
                          ویرایش
                        </a>
                        <a href="javascript:void(0)" class="deleteBtn">
                          حذف
                        </a>
                      </td>
                    </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <form>
                <div class="row submit-cancel">
                  <nuxt-link class="submit" :to="`/profile/residence/edit/facilities/`+id">
                    <i class="bx bx-chevron-right"></i>
                    مرحله بعد
                  </nuxt-link>
                  <nuxt-link class="cancel" :to="`/profile/residence/edit/images/`+id">مرحله قبل</nuxt-link>
                </div>
              </form>
            </div>
          </div>
        </div>
        <vk-modal center size="container" id="editModalNearbyPlace" class="modal-info-submit" :show.sync="editModalNearbyPlace">
          <div class="title">
            <p>
              بروزرسانی مکان
            </p>
            <vk-modal-close @click="editModalNearbyPlace = false"></vk-modal-close>
          </div>
          <div class="text">
            <div class="row">
              <div class="form-group w-30">
                <input type="text" id="utitle" v-model="editNearByPlace.title" required>
                <span class="floating-label">نام مکان مورد نظر</span>
              </div>
              <div class="form-group w-30">
                <input type="text" id="udistanceWalk" v-model="editNearByPlace.distanceWalk" required>
                <span class="floating-label">فاصله زمانی پیاده (دقیقه)</span>
              </div>
              <div class="form-group w-30">
                <input type="text" id="udistanceCar" v-model="editNearByPlace.distanceCar" required>
                <span class="floating-label">فاصله زمانی با خودرو (دقیقه)</span>
              </div>
            </div>
            <div class="submit-cancel">
              <button type="button" class="submit" v-if="updating === false" @click="updateNearbyPlace">
                <i class="bx bx-chevron-right"></i>
                اعمال
              </button>
              <button type="button" class="submit" v-else>
                <i class="bx bx-chevron-right"></i>
                چند لحظه صبر کنید
              </button>
            </div>
          </div>
        </vk-modal>
      </section>
    </main>
  </div>
</template>

<style src="~/assets/styles/css/newResidence2.css" scoped />

<script>
  import Header from '~/components/Header';
  import Footer from '~/components/Footer';
  import { mapGetters } from 'vuex';

  export default {
    layout: 'profile',
    components: {
      Header,
      Footer
    },
    data() {
      return {
        id: 0,
        roadOptions: [],
        environmentalOptions: [],
        nearByPlaces: [],
        menu: true,
        nearByPlace: {
          title: null,
          distanceWalk: null,
          distanceCar: null
        },
        editNearByPlace: {
          id: null,
          title: null,
          distanceWalk: null,
          distanceCar: null
        },
        editModalNearbyPlace: false,
        creating: false,
        updating: false
      }
    },
    async asyncData({params,$auth,$axios,redirect}) {
      if(!$auth.loggedIn)
        redirect('/');

      const {data} = await $axios.post('api/v1/user/residence/editRoadOptions',{id:params.id});

      return {
        id:params.id,
        roadOptions: data.roadOptions,
        environmentalOptions: data.environmentalOptions,
        nearByPlaces: data.nearByPlaces
      };
    },
    methods: {
      async createNearbyPlace() {
        this.creating = true;
        this.errors = [];
        const saveNearByPlace = await this.$axios.post('api/v1/user/residence/saveNearByPlace',
          {id: this.id,title: this.nearByPlace.title, distanceWalk: this.nearByPlace.distanceWalk, distanceCar: this.nearByPlace.distanceCar});
        if(saveNearByPlace.data.status === 1) {
          this.$snotify.success(saveNearByPlace.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          const data = await this.$axios.post('api/v1/user/residence/editRoadOptions',{id:this.id});
          this.roadOptions = data.data.roadOptions;
          this.environmentalOptions = data.data.environmentalOptions;
          this.nearByPlaces = data.data.nearByPlaces;
          this.nearByPlace = {
            title: null,
            distanceWalk: null,
            distanceCar: null
          };
        } else if(saveNearByPlace.data.status === 422) {
          this.errors = saveNearByPlace.data.errors;
          this.$snotify.warning(saveNearByPlace.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(saveNearByPlace.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.creating = false;
      },
      async updateNearbyPlace() {
        this.updating = true;
        this.errors = [];
        const updateNearByPlace = await this.$axios.post('api/v1/user/residence/updateNearByPlace',
          {id: this.editNearByPlace.id,title: this.editNearByPlace.title, distanceWalk: this.editNearByPlace.distanceWalk, distanceCar: this.editNearByPlace.distanceCar});
        if(updateNearByPlace.data.status === 1) {
          this.$snotify.success(updateNearByPlace.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.editModalNearbyPlace = false;
          const data = await this.$axios.post('api/v1/user/residence/editRoadOptions',{id:this.id});
          this.roadOptions = data.data.roadOptions;
          this.environmentalOptions = data.data.environmentalOptions;
          this.nearByPlaces = data.data.nearByPlaces;
          this.editNearByPlace = {
            id: null,
            title: null,
            distanceWalk: null,
            distanceCar: null
          };
        } else if(updateNearByPlace.data.status === 422) {
          this.errors = updateNearByPlace.data.errors;
          this.$snotify.warning(updateNearByPlace.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(updateNearByPlace.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.updating = false;
      },
      async changeRoadOption(roadOption) {
        let ar = [];
        roadOption.checked = !roadOption.checked;
        this.roadOptions.forEach(element => {
          if(element.checked !== false)
            ar.push(element.id);
        });
        await this.$axios.post('api/v1/user/residence/updateRoadOptions/'+this.id,{options:ar});
      },
      async changeEnvironmentalOption(environmentalOption) {
        let ar = [];
        environmentalOption.checked = !environmentalOption.checked;
        this.environmentalOptions.forEach(element => {
          if(element.checked !== false)
            ar.push(element.id);
        });
        await this.$axios.post('api/v1/user/residence/updateEnvironmentalOptions/'+this.id,{options:ar});
      },
      setNearbyPlace(nearbyPlace) {
        this.editNearByPlace = nearbyPlace;
        this.editModalNearbyPlace = true;
      },
      closeMenu() {
        const main = this.$refs.main;
        if (main.classList.contains('blur')) {
          this.$store.dispatch('setVuexMenuState',false);
          this.$store.dispatch('setVuexLogregMobileState',false);
        }
      },
      openPageMenu() {
        this.menu = !this.menu;
      },
      number_3_3(num, sep) {
        const number = typeof num === 'number' ? num.toString() : num;
        const separator = typeof sep === 'undefined' ? ',' : sep;
        if(typeof number !== 'undefined')
          return number.replace(/(\d)(?=(\d\d\d)+(?!\d))/g, '$1' + separator);
      }
    },
    computed: {
      vuexMenuState() {
        return this.$store.getters.vuexMenuState
      },
      vuexLogregMobileState() {
        return this.$store.getters.vuexLogregMobileState
      },
      ...mapGetters(['settings'])
    },
    created() {
      this.$store.dispatch('setVuexMenuState',false);
      this.$store.dispatch('setVuexProfileMenuState',false);
    },
    head() {
      return {
        title: 'مدیریت اقامتگاه - '+this.settings.sitename,
        meta: [
          {
            hid: 'description',
            name: 'description',
            content: this.settings.sitedesc
          },
        ],
      }
    }
  }
</script>
