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
                <li class="active">
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
              <p class="form-name">تصاویر اقامتگاه</p>
              <p class="form-description">
                تصاویر با کیفیت مربوط به اقامتگاه را آپلود کنید تا در دسترس میهمان قرار گیرد. پیشنهاد می‌کنیم تصاویر را از تمام اتاق‌ها، سرویس‌های بهداشتی، حیاط، باغ و ... تهیه و ارسال فرمایید.
              </p>
              <form>
                <div class="main-img">
                  <div class="img-title">
                    <p>تصویر اصلی اقامگاه (در صفحه اصلی سایت و جستجو نمایش داده می‌شود.</p>
                    <input type="file" @change="uploadMainImage" ref="mainImageInput" style="display: none;">
                    <button type="button" @click="$refs.mainImageInput.click()"
                            v-if="uploadingMainImagePercent === 0">انتخاب تصویر اصلی</button>
                    <button type="button" v-else>در حال آپلود : {{ uploadingMainImagePercent }}%</button>
                  </div>
                  <div class="show-img">
                    <img :src="image" alt="">
                  </div>
                </div>
                <div class="thumbnail-img">
                  <div class="img-title">
                    <p>آلبوم تصاویر اقامتگاه</p>
                    <input type="file" @change="uploadAlbumImage" ref="albumImageInput" style="display: none;">
                    <button type="button" @click="$refs.albumImageInput.click()"
                            v-if="uploadingAlbumImagePercent === 0">افزودن تصویر به آلبوم</button>
                    <button type="button" v-else>در حال آپلود : {{ uploadingAlbumImagePercent }}%</button>
                  </div>
                  <div class="show-img">
                    <div v-for="albumImage in albumImages">
                      <img :src="albumImage" alt="">
                      <a href="javascript:void(0)" @click="deleteAlbumImage(albumImage)" v-if="deleting === false"><i class="bx bx-trash"></i></a>
                      <a href="javascript:void(0)" v-else><i class="bx bx-trash"></i></a>
                    </div>
                  </div>
                </div>

                <div class="row submit-cancel uk-margin-top">
                  <nuxt-link class="submit" :to="`/profile/residence/edit/roadOptions/`+id">
                    <i class="bx bx-chevron-right"></i>
                    مرحله بعد
                  </nuxt-link>
                  <nuxt-link class="cancel" :to="`/profile/residence/edit/calendar/`+id">مرحله قبل</nuxt-link>
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
        albumImages: [],
        image: null,
        menu: true,
        deleting: false,
        uploadingMainImagePercent: 0,
        uploadingAlbumImagePercent: 0
      }
    },
    async asyncData({params,$auth,$axios,redirect}) {
      if(!$auth.loggedIn)
        redirect('/');

      const {data} = await $axios.post('api/v1/user/residence/editImages',{id:params.id});

      return {
        id:params.id,
        albumImages: data.images.albumImages,
        image: data.images.image
      };
    },
    methods: {
      uploadMainImage(e) {
        const fd = new FormData();
        fd.append('id',this.id);
        fd.append('image',e.target.files[0]);
        this.$axios.post('api/v1/user/residence/uploadMainImage',fd,{
          onUploadProgress: uploadEvent => {
            this.uploadingMainImagePercent = Math.round(uploadEvent.loaded / uploadEvent.total * 100);
          }
        })
        .then(res=>{
          if(res.data.status === 1) {
            this.$snotify.success('چند لحظه صبر کنید', {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
            this.$router.go();
          } else {
            this.uploadingMainImagePercent = 0;
            this.$snotify.error(res.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          }
        });
      },
      uploadAlbumImage(e) {
        const fd = new FormData();
        fd.append('id',this.id);
        fd.append('image',e.target.files[0]);
        this.$axios.post('api/v1/user/residence/uploadAlbumImage',fd,{
          onUploadProgress: uploadEvent => {
            this.uploadingAlbumImagePercent = Math.round(uploadEvent.loaded / uploadEvent.total * 100);
          }
        })
          .then(res=>{
            if(res.data.status === 1) {
              this.$snotify.success('چند لحظه صبر کنید', {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
              this.$router.go();
            } else {
              this.uploadingAlbumImagePercent = 0;
              this.$snotify.error(res.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
            }
          });
      },
      async deleteAlbumImage(albumImage) {
        this.deleting = true;
        const deleteAlbumImage = await this.$axios.post('api/v1/user/residence/deleteAlbumImage', {id:this.id,  name: albumImage});
        if(deleteAlbumImage.data.status === 1) {
          this.$snotify.success('چند لحظه صبر کنید', {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.$router.go();
        } else {
          this.$snotify.error(deleteAlbumImage.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
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
