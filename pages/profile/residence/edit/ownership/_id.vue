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
        <div class="body">
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
                <li>
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
                <li class="active">
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
              <p class="form-name">اطلاعات مالکیت</p>
              <p class="form-description">
                اطلاعات و مدارک مربوط به مالکیت اقامتگاه را وارد کنید. ارسال مدارک در حال حاضر اجباری نیست و شما می‌توانید تا زمان آماده کردن مدارک مراحل دیگر افزودن اقامتگاه را تکمیل کنید. توجه داشته باشید که مدارک شما پس از رویت کارشناسان از سرورهای "بروبریم" حذف می‌شوند و در مکانی امن و خارج از دسترس نگهداری خواهند شد.
              </p>
              <form action="">
                <div class="select-owner">
                  <a href="javascript:void(0)" @click="changeOwnershipType('واسط')" v-bind:class="[ownership.type === 'واسط' ? 'active' : '']">واسط</a>
                  <a href="javascript:void(0)" @click="changeOwnershipType('آشنای مالک')" v-bind:class="[ownership.type === 'آشنای مالک' ? 'active' : '']">آشنای مالک</a>
                  <a href="javascript:void(0)" @click="changeOwnershipType('مالک')" v-bind:class="[ownership.type === 'مالک' ? 'active' : '']">مالک</a>
                  <a href="javascript:void(0)" @click="changeOwnershipType('سرایدار / نگهبان')" v-bind:class="[ownership.type === 'سرایدار / نگهبان' ? 'active' : '']">سرایدار / نگهبان</a>
                  <a href="javascript:void(0)" @click="changeOwnershipType('مستاجر بلند مدت')" v-bind:class="[ownership.type === 'مستاجر بلند مدت' ? 'active' : '']">مستاجر بلند مدت</a>
                </div>

                <div class="upload-img">
                  <div>
                    <img :src="ownership.imageYourCard" alt="">
                    <input type="file" @change="uploadYourCard" ref="yourCardInput" style="display: none;">
                    <button type="button" @click="$refs.yourCardInput.click()"
                            v-if="uploadingYourCardPercent === 0">آپلود تصویر کارت ملی شما</button>
                    <button type="button" v-else>در حال آپلود : {{ uploadingYourCardPercent }}%</button>

                  </div>
                  <div>
                    <img :src="ownership.imageOwnerCard" alt="">
                    <input type="file" @change="uploadOwnerCard" ref="ownerCardInput" style="display: none;">
                    <button type="button" @click="$refs.ownerCardInput.click()"
                            v-if="uploadingOwnerCardPercent === 0">آپلود تصویر کارت ملی مالک</button>
                    <button type="button" v-else>در حال آپلود : {{ uploadingOwnerCardPercent }}%</button>
                  </div>
                  <div>
                    <img :src="ownership.imageSanad" alt="">
                    <input type="file" @change="uploadSanad" ref="sanadInput" style="display: none;">
                    <button type="button" @click="$refs.sanadInput.click()"
                            v-if="uploadingSanadPercent === 0">آپلود تصویر سند قبض یا یکی از فیش‌های برق، تلفن یا آب</button>
                    <button type="button" v-else>در حال آپلود : {{ uploadingSanadPercent }}%</button>
                  </div>
                </div>

                <div class="row uk-margin-top submit-cancel">
                  <button class="submit" type="button" v-if="updating === false" @click="updateOwnership">
                    مرحله بعد
                  </button>
                  <button class="submit" type="button" v-else>
                    چند لحظه صبر کنید
                  </button>
                  <nuxt-link class="cancel" :to="`/profile/residence/edit/rooms/`+id">مرحله قبل</nuxt-link>
                </div>

              </form>
            </div>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex';

  export default {
    layout: 'profile',
    data() {
      return {
        id: 0,
        ownership: [],
        menu: true,
        errors: [],
        updating: false,
        uploadingYourCardPercent: 0,
        uploadingOwnerCardPercent: 0,
        uploadingSanadPercent: 0
      }
    },
    async asyncData({params,$auth,$axios,redirect}) {
      if(!$auth.loggedIn)
        redirect('/');

      const {data} = await $axios.post('api/v1/user/residence/editOwnership',{id:params.id});

      return {
        id: params.id,
        ownership: data.ownership
      };
    },
    methods: {
      uploadYourCard(e) {
        const fd = new FormData();
        fd.append('id',this.id);
        fd.append('image',e.target.files[0]);
        this.$axios.post('api/v1/user/residence/uploadYourCard',fd,{
          onUploadProgress: uploadEvent => {
            this.uploadingYourCardPercent = Math.round(uploadEvent.loaded / uploadEvent.total * 100);
          }
        })
          .then(res=>{
            if(res.data.status === 1) {
              this.$snotify.success('چند لحظه صبر کنید', {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
            }else {
              this.$snotify.error('خطا در ذخیره تصویر', {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
            }
            this.$router.go();
          });
      },
      uploadOwnerCard(e) {
        const fd = new FormData();
        fd.append('id',this.id);
        fd.append('image',e.target.files[0]);
        this.$axios.post('api/v1/user/residence/uploadOwnerCard',fd,{
          onUploadProgress: uploadEvent => {
            this.uploadingOwnerCardPercent = Math.round(uploadEvent.loaded / uploadEvent.total * 100);
          }
        })
          .then(res=>{
            if(res.data.status === 1) {
              this.$snotify.success('چند لحظه صبر کنید', {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
            }else {
              this.$snotify.error('خطا در ذخیره تصویر', {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
            }
            this.$router.go();
          });
      },
      uploadSanad(e) {
        const fd = new FormData();
        fd.append('id',this.id);
        fd.append('image',e.target.files[0]);
        this.$axios.post('api/v1/user/residence/uploadSanad',fd,{
          onUploadProgress: uploadEvent => {
            this.uploadingSanadPercent = Math.round(uploadEvent.loaded / uploadEvent.total * 100);
          }
        })
          .then(res=>{
            if(res.data.status === 1) {
              this.$snotify.success('چند لحظه صبر کنید', {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
            }else {
              this.$snotify.error('خطا در ذخیره تصویر', {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
            }
            this.$router.go();
          });
      },
      async updateOwnership() {
        this.updating = true;
        this.errors = [];
        const updateOwnership = await this.$axios.post('api/v1/user/residence/updateOwnership',{id:this.id,type:this.ownership.type});
        if(updateOwnership.data.status === 1) {
          this.$snotify.success(updateOwnership.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.$router.push('/profile/residence/edit/personal/'+this.id);
        } else if(updateOwnership.data.status === 422) {
          this.errors = updateOwnership.data.errors;
          this.$snotify.warning(updateOwnership.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(updateOwnership.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.updating = false;
      },
      changeOwnershipType(type) {
        this.ownership.type = type;
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
