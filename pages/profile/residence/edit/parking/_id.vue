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
                <li class="active">
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
              <p class="form-name">پارکینگ</p>
              <p class="form-description">
                اطلاعات مربوط به پارکینگ خودرو در اقامتگاه‌تان را وارد فرمایید.
              </p>
              <form>
                <div class="row">
                  <div class="form-group checkbox w-50">
                    <input type="checkbox" class="uk-checkbox" id="parkingOutside" v-model="parking.outside">
                    <label for="parkingOutside">جای پارکینگ در بیرون از اقامتگاه است</label>
                  </div>
                  <div class="form-group checkbox w-50">
                    <input type="checkbox" class="uk-checkbox" id="parkingPublic" v-model="parking.public">
                    <label for="parkingPublic">پارکینگ عمومی در نزدیکی اقامتگاه وجود دارد</label>
                  </div>
                </div>
                <div class="row">
                  <div class="form-group w-100">
                    <label for="parkingText">توضیحات پارکینگ</label>
                    <textarea id="parkingText" rows="7" v-model="parking.text"></textarea>
                    <span v-if="errors.parkingText" class="error">{{ errors.parkingText[0] }}</span>
                  </div>
                </div>
                <hr>
                <br>
                <div class="row">
                  <p class="form-name w-100 uk-margin-medium-bottom">ثبت پارکینگ جدید</p>
                  <div class="form-group w-50">
                    <select class="uk-select" id="type" v-model="newForm.type" style="padding: 0 0;">
                      <option :value="null">نوع پارکینگ</option>
                      <option :value="1">سرپوشیده</option>
                      <option :value="2">روباز</option>
                    </select>
                    <span v-if="errors.type" class="error">{{ errors.type[0] }}</span>
                  </div>
                  <div class="form-group w-50">
                    <input type="text" id="count" v-model="newForm.count" required>
                    <span class="floating-label">ظرفیت پارکینگ</span>
                    <span v-if="errors.count" class="error">{{ errors.count[0] }}</span>
                  </div>
                  <div class="form-group checkbox w-50">
                    <input type="checkbox" class="uk-checkbox" id="busy" v-model="newForm.busy" :checked="newForm.busy">
                    <label for="busy">پارکینگ مزاحم است</label>
                  </div>
                  <div class="form-group checkbox w-50">
                    <input type="checkbox" class="uk-checkbox" id="tiny" v-model="newForm.tiny" :checked="newForm.tiny">
                    <label for="tiny">پارکینگ مناسب خودرو‌های شاسی بلند</label>
                  </div>
                </div>
                <div class="row submit-cancel uk-margin-top uk-margin-bottom">
                  <button type="button" class="submit" v-if="creating === false" @click="createParking">ثبت پارکینگ جدید</button>
                  <button type="button" class="submit" v-else>چند لحظه صبر کنید</button>
                </div>
              </form>
              <br>
              <hr>
              <br>
              <div class="show-lists">
                <p class="form-name w-100 uk-margin-medium-bottom">پارکینگ ها</p>
                <div class="uk-overflow-auto">
                  <table class="uk-table uk-table-middle">
                    <thead>
                    <tr>
                      <th>#</th>
                      <th>نوع</th>
                      <th>تعداد</th>
                      <th>عملیات</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(parking,index) in parking.parkings">
                      <td class="date" style="padding-right: 15px;">{{ index+1 }}</td>
                      <td class="date">
                        <span v-if="parking.type === 1">سرپوشیده</span>
                        <span v-if="parking.type === 2">روباز</span>
                      </td>
                      <td class="date">{{ parking.count }}</td>
                      <td class="more-action">
                        <a href="javascript:void(0)" class="editBtn" @click="setParking(parking)">
                          ویرایش
                        </a>
                        <a href="javascript:void(0)" class="deleteBtn" @click="deleteParking(parking.id)" v-if="deleting === false">
                          حذف
                        </a>
                        <a href="javascript:void(0)" class="cancel" v-else>
                          <span class="name">چند لحظه صبر کنید</span>
                        </a>
                      </td>
                    </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <form>
                <div class="row submit-cancel uk-margin-top">
                  <button class="submit" type="button" v-if="updating === false" @click="updateParkingDetail">
                    مرحله بعد
                  </button>
                  <button class="submit" type="button" v-else>
                    چند لحظه صبر کنید
                  </button>
                  <nuxt-link class="cancel" :to="`/profile/residence/edit/personal/`+id">مرحله قبل</nuxt-link>
                </div>
              </form>
            </div>
          </div>
        </div>
        <vk-modal center id="editModalParking" class="modal-info-submit" :show.sync="editModalParking">
          <div class="title">
            <p>
              بروزرسانی پارکینگ
            </p>
            <vk-modal-close @click="editModalParking = false"></vk-modal-close>
          </div>
          <div class="text">
            <div class="row">
              <div class="form-group uk-margin-bottom w-100">
                <select class="uk-select" id="utype" v-model="editParking.type" style="padding: 0 0;">
                  <option :value="1">سرپوشیده</option>
                  <option :value="2">روباز</option>
                </select>
                <span v-if="errors.type" class="error">{{ errors.type[0] }}</span>
              </div>
              <div class="form-group w-100">
                <input type="text" id="ucount" v-model="editParking.count" required>
                <span class="floating-label">تعداد</span>
                <span v-if="errors.count" class="error">{{ errors.count[0] }}</span>
              </div>
              <div class="checkbox checkbox-with-input w-50">
                <div>
                  <input type="checkbox" class="uk-checkbox" id="ubusy" v-model="editParking.busy" :checked="editParking.busy">
                  <label for="ubusy">پارکینگ مزاحم است</label>
                </div>
              </div>
              <div class="checkbox checkbox-with-input w-50">
                <div>
                  <input type="checkbox" class="uk-checkbox" id="utiny" v-model="editParking.tiny" :checked="editParking.tiny">
                  <label for="utiny">پارکینگ مناسب خودرو‌های شاسی‌بلند</label>
                </div>
              </div>
            </div>
            <div class="submit-cancel">
              <button type="button" class="submit" v-if="updating === false" @click="updateParking">
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
  import { mapGetters } from 'vuex';

  export default {
    layout: 'profile',
    data() {
      return {
        id: 0,
        newForm: {
          id: 0,
          type: null,
          count: 0,
          busy: false,
          tiny: false
        },
        editParking: {
          type: 1,
          count: 0,
          busy: false,
          tiny: false
        },
        parking: [],
        errors: [],
        editModalParking: false,
        creating: false,
        deleting: false,
        updating: false
      }
    },
    async asyncData({params,$auth,$axios,redirect}) {
      if(!$auth.loggedIn)
        redirect('/');

      const {data} = await $axios.post('api/v1/user/residence/editParkingDetail',{id:params.id});

      return {
        id: params.id,
        parking: data.parking,
        menu: true,
        newForm: {
          id: params.id,
          type: null,
          count: 0,
          busy: false,
          tiny: false
        },
      };
    },
    methods: {
      setParking(parking) {
        this.editParking = parking;
        this.editModalParking = true;
      },
      async createParking() {
        this.creating = true;
        this.errors = [];
        const createParking = await this.$axios.post('api/v1/user/residence/saveParking',this.newForm);
        if(createParking.data.status === 1) {
          this.$snotify.success(createParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.$router.go();
        } else if(createParking.data.status === 422) {
          this.errors = createParking.data.errors;
          this.$snotify.warning(createParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(createParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.creating = false;
      },
      async updateParking() {
        this.updating = true;
        this.errors = [];
        const updateParking = await this.$axios.post('api/v1/user/residence/updateParking',this.editParking);
        if(updateParking.data.status === 1) {
          this.$snotify.success(updateParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.editModalParking = false;
          this.$router.go();
        } else if(updateParking.data.status === 422) {
          this.errors = updateParking.data.errors;
          this.$snotify.warning(updateParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(updateParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.updating = false;
      },
      async deleteParking(id) {
        this.deleting = true;
        this.errors = [];
        const deleteParking = await this.$axios.post('api/v1/user/residence/deleteParking',{'id':id});
        if(deleteParking.data.status === 1) {
          this.$snotify.success(deleteParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          const parking = await this.$axios.post('api/v1/user/residence/editParkingDetail',{id:this.id});
          this.parking = parking.data.parking;
        } else if(deleteParking.data.status === 422) {
          this.errors = deleteParking.data.errors;
          this.$snotify.warning(deleteParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(deleteParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.deleting = false;
      },
      async updateParkingDetail() {
        this.updating = true;
        this.errors = [];
        const updateParking = await this.$axios.post('api/v1/user/residence/updateParkingDetail',
          {'id':this.id,'outside':this.parking.outside,'public':this.parking.public,'text':this.parking.text});
        if(updateParking.data.status === 1) {
          this.$snotify.success(updateParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.$router.push('/profile/residence/edit/map/'+this.id);
        } else if(updateParking.data.status === 422) {
          this.errors = updateParking.data.errors;
          this.$snotify.warning(updateParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(updateParking.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.updating = false;
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
