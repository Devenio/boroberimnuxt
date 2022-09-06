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
                <li class="active">
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
              <p class="form-name">تقویم کاری</p>
              <p class="form-description">
                با استفاده از تقویم کاری می‌توانید روزهای دلخواه را برای رزرو از دسترس خارج کنید، قیمت بعضی از روز‌ها را تغییر دهید یا قابلیت رزرو فوری را برای روزهایی که می‌خواهید فعال کنید.
              </p>
              <form>
                <div style="box-sizing: border-box !important;">
                  <datepicker
                    :forward-limit="{ year: settings.nextMonth.year, month: settings.nextMonth.month }"
                    :backward-limit="{ year: settings.previousMonth.year, month: settings.previousMonth.month }"
                    :disabled-map="{}"
                    :holiday-map="{}"
                    lang="Jalali"
                    type="range"
                    size="lg"
                    :debug-selector="false"
                    :enable-all-days="true"
                    :days="calendars"
                    :defPrice="priceDefault"
                    :holidayPrice="priceVacation"
                    @datemodel="handledate"
                  />
                </div>
                <div class="row submit-cancel uk-margin-top">
                  <nuxt-link class="submit" :to="`/profile/residence/edit/images/`+id">
                    <i class="bx bx-chevron-right"></i>
                    مرحله بعد
                  </nuxt-link>
                  <button class="submit" type="button" @click="modalInfoSubmit = true" v-if="date.dates.length > 0">
                    ویرایش روزهای انتخاب شده
                  </button>
                  <nuxt-link class="cancel" :to="`/profile/residence/edit/detail/`+id">مرحله قبل</nuxt-link>
                </div>
              </form>
            </div>
          </div>
        </div>
        <vk-modal center id="modal-info-submit" class="modal-info-submit" :show.sync="modalInfoSubmit">
          <div class="title">
            <p>
              <i class="bx bx-doughnut-chart"></i>
              ویرایش روزهای انتخاب شده
            </p>
            <vk-modal-close @click="modalInfoSubmit = false"></vk-modal-close>
          </div>
          <div class="text">
            <div class="house-kind">
              <p>
                <i class="bx bx-dots-horizontal-rounded"></i>
                وضعیت
              </p>
              <div class="items">
                <a href="javascript:void(0)" v-bind:class="(status === 0)?'active':''" @click="changeStatus(0)">عدم تغییر</a>
                <a href="javascript:void(0)" v-bind:class="(status === 1)?'active':''" @click="changeStatus(1)">پر/غیرفعال</a>
                <a href="javascript:void(0)" v-bind:class="(status === 2)?'active':''" @click="changeStatus(2)">خالی/در دسترس</a>
                <a href="javascript:void(0)" v-bind:class="(status === 3)?'active':''" @click="changeStatus(3)">قابلیت رزرو فوری</a>
              </div>
            </div>
            <div class="row uk-margin-top">
              <div class="form-group w-50">
                <input type="text" id="price" v-model="price" required>
                <span class="floating-label">قیمت روزهای انتخاب شده (هزار تومان) :</span>
              </div>
              <div class="form-group w-50">
                <input type="text" id="reserveDays" v-model="reserveDays" required>
                <span class="floating-label">حداقل مدت رزرو در روزهای انتخاب شده :</span>
              </div>
            </div>
            <div class="submit-cancel">
              <button type="button" class="submit" v-if="updating === false" @click="updateCalendar">
                <i class="bx bx-chevron-right"></i>
                اعمال
              </button>
              <button type="button" class="submit" v-else>
                <i class="bx bx-chevron-right"></i>
                چند لحظه صبر کنید
              </button>
              <button class="cancel" @click="modalInfoSubmit = false">انصراف</button>
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
  import datepicker from '@/components/datepicker2';

  export default {
    layout: 'profile',
    components: {
      datepicker
    },
    data() {
      return {
        id: 0,
        calendars: [],
        date: {dates:[]},
        status: 0,
        price: null,
        reserveDays: null,
        menu: true,
        errors: [],
        priceDefault: 0,
        priceVacation: 0,
        modalInfoSubmit: false,
        updating: false
      }
    },
    async asyncData({params,$auth,$axios,redirect}) {
      if(!$auth.loggedIn)
        redirect('/');

      const {data} = await $axios.post('api/v1/user/residence/editCalendar',{id:params.id});

      return {
        id:params.id,
        calendars: data.calendars,
        priceDefault: data.priceDefault,
        priceVacation: data.priceVacation
      };
    },
    methods: {
      handledate(e) {
        this.date = e;
      },
      changeStatus(number) {
        this.status = number;
      },
      async updateCalendar() {
        this.updating = true;
        this.errors = [];
        let startDate = null;
        let endDate = null;
        if(this.date.dates.length > 0) {
          startDate = this.date.dates[0]['year']+"-"+this.date.dates[0]['month']+"-"+this.date.dates[0]['date'];
          endDate = this.date.dates[1]['year']+"-"+this.date.dates[1]['month']+"-"+this.date.dates[1]['date'];
        }
        const updateCalendar = await this.$axios.post('api/v1/user/residence/updateCalendar',
          {id:this.id,  status: this.status, price: this.price, reserveDays: this.reserveDays,startDate:startDate,endDate:endDate});
        if(updateCalendar.data.status === 1) {
          this.$snotify.success('چند لحظه صبر کنید', {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.$router.go();
        } else if(updateCalendar.data.status === 422) {
          this.errors = updateCalendar.data.errors;
          this.$snotify.warning(updateCalendar.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(updateCalendar.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
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
      dateObj() {
        return this.date?.dates
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
