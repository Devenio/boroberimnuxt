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
                <li class="active">
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
              <p class="form-name"> اطلاعات جزئی اقامتگاه</p>
              <p class="form-description">
                اطلاعات ثانویه اقامتگاه‌تان را مانند ظرفیت اقامتگاه، ساعات خروج و ورود مهمان و ... را وارد کنید تا قوانین رزرو اقامتگاه شما تکمیل گردد.
              </p>
              <form action="" class="form-submit">
                <no-ssr>
                  <div class="row">
                    <div class="form-group w-50">
                      <input type="text" id="allFloor" v-model="details.allFloor" required>
                      <span class="floating-label">تعداد کل طبقات (برای اقامتگاه ویلایی 0 وارد کنید)</span>
                      <span v-if="errors.allFloor" class="error">{{ errors.allFloor[0] }}</span>
                    </div>
                    <div class="form-group w-50">
                      <input type="text" id="floor" v-model="details.floor" required>
                      <span class="floating-label">طبقه واحد (برای طبقه همکف 0 وارد کنید)</span>
                      <span v-if="errors.floor" class="error">{{ errors.floor[0] }}</span>
                    </div>
                  </div>
                  <div class="row">
                    <div class="form-group w-30">
                      <input type="text" id="startDeliveryTime" v-model="details.startDeliveryTime" @click="chooseClock(1)" required>
                      <span class="floating-label">ساعت تحویل از</span>
                      <span v-if="errors.startDeliveryTime" class="error">{{ errors.startDeliveryTime[0] }}</span>
                    </div>
                    <div class="form-group w-30">
                      <input type="text" id="endDeliveryTime" v-model="details.endDeliveryTime" @click="chooseClock(2)" required>
                      <span class="floating-label">ساعت تحویل تا</span>
                      <span v-if="errors.endDeliveryTime" class="error">{{ errors.endDeliveryTime[0] }}</span>
                    </div>
                    <div class="form-group w-30">
                      <input type="text" id="evacuateTime" v-model="details.evacuateTime" @click="chooseClock(3)" required>
                      <span class="floating-label">ساعت خروج</span>
                      <span v-if="errors.evacuateTime" class="error">{{ errors.evacuateTime[0] }}</span>
                    </div>
                  </div>
                  <div class="row">
                    <div class="form-group w-50">
                      <input type="text" id="minReserve" v-model="details.minReserve" required>
                      <span class="floating-label">حداقل تعداد روز هر رزرو</span>
                      <span v-if="errors.minReserve" class="error">{{ errors.minReserve[0] }}</span>
                    </div>
                    <div class="form-group w-50">
                      <input type="text" id="maxReserve" v-model="details.maxReserve" required>
                      <span class="floating-label">حداکثر تعداد روز هر رزرو</span>
                      <span v-if="errors.maxReserve" class="error">{{ errors.maxReserve[0] }}</span>
                    </div>
                  </div>
                  <div class="row">
                    <div class="form-group w-50">
                      <input type="text" id="minPerson" v-model="details.minPerson" required>
                      <span class="floating-label">ظرفیت استاندارد اقامتگاه</span>
                      <span v-if="errors.minPerson" class="error">{{ errors.minPerson[0] }}</span>
                    </div>
                    <div class="form-group w-50">
                      <input type="text" id="maxPerson" v-model="details.maxPerson" required>
                      <span class="floating-label">حداکثر ظرفیت اقامتگاه</span>
                      <span v-if="errors.maxPerson" class="error">{{ errors.maxPerson[0] }}</span>
                    </div>
                  </div>
                  <div class="row">
                    <div class="form-group w-30">
                      <input type="text" id="age" v-model="details.age" required>
                      <span class="floating-label">سن بنا</span>
                      <span v-if="errors.age" class="error">{{ errors.age[0] }}</span>
                    </div>
                    <div class="form-group w-30">
                      <input type="text" id="room" v-model="details.room" required>
                      <span class="floating-label">تعداد اتاق‌ها</span>
                      <span v-if="errors.room" class="error">{{ errors.room[0] }}</span>
                    </div>
                    <div class="form-group w-30">
                      <input type="text" id="rebuildYear" v-model="details.rebuildYear" required>
                      <span class="floating-label">سال بازسازی (اختیاری)</span>
                      <span v-if="errors.rebuildYear" class="error">{{ errors.rebuildYear[0] }}</span>
                    </div>
                  </div>
                  <div class="row">
                    <div class="form-group w-30">
                      <label for="irancellCoverage">آنتن‌دهی ایرانسل</label>
                      <select class="uk-select" id="irancellCoverage" v-model="details.irancellCoverage" style="padding: 0 0;">
                        <option>ضعیف</option>
                        <option>متوسط</option>
                        <option>عالی</option>
                      </select>
                      <span v-if="errors.irancellCoverage" class="error">{{ errors.irancellCoverage[0] }}</span>
                    </div>
                    <div class="form-group w-30">
                      <label for="raitellCoverage">آنتن‌دهی رایتل</label>
                      <select class="uk-select" id="raitellCoverage" v-model="details.raitellCoverage" style="padding: 0 0;">
                        <option>ضعیف</option>
                        <option>متوسط</option>
                        <option>عالی</option>
                      </select>
                      <span v-if="errors.raitellCoverage" class="error">{{ errors.raitellCoverage[0] }}</span>
                    </div>
                    <div class="form-group w-30">
                      <label for="hamrahavalCoverage">آنتن‌دهی همراه‌اول</label>
                      <select class="uk-select" id="hamrahavalCoverage" v-model="details.hamrahavalCoverage" style="padding: 0 0;">
                        <option>ضعیف</option>
                        <option>متوسط</option>
                        <option>عالی</option>
                      </select>
                      <span v-if="errors.hamrahavalCoverage" class="error">{{ errors.hamrahavalCoverage[0] }}</span>
                    </div>
                  </div>
                  <div class="row">
                    <div class="form-group w-100">
                      <textarea id="masterText" v-model="details.text" required></textarea>
                      <span class="floating-label">توضیحات اقامتگاه</span>
                      <span v-if="errors.text" class="error">{{ errors.text[0] }}</span>
                    </div>
                  </div>
                  <br><br>
                </no-ssr>
                <div class="row submit-cancel uk-margin-top">
                  <button class="submit" v-if="updating === false" @click="updateDetail">
                    <i class="bx bx-chevron-right"></i>
                    مرحله بعد
                  </button>
                  <button class="submit" type="button" v-else>
                    چند لحظه صبر کنید
                  </button>
                  <nuxt-link class="cancel" :to="`/profile/residence/edit/`+id">مرحله قبل</nuxt-link>
                </div>
              </form>
            </div>
          </div>
        </div>
        <vk-modal center id="clockModal" class="modal-info-submit" :show.sync="clockModal">
          <div class="title">
            <p>
              انتخاب ساعت
            </p>
            <vk-modal-close @click="clockModal = false"></vk-modal-close>
          </div>
          <div class="text">
            <div class="row">
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-secondary" @click="setClock('8:00')">8:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-secondary" @click="setClock('9:00')">9:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-primary" @click="setClock('10:00')">10:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-primary" @click="setClock('11:00')">11:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-secondary" @click="setClock('12:00')">12:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-secondary" @click="setClock('13:00')">13:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-primary" @click="setClock('14:00')">14:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-primary" @click="setClock('15:00')">15:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-secondary" @click="setClock('16:00')">16:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-secondary" @click="setClock('17:00')">17:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-primary" @click="setClock('18:00')">18:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-primary" @click="setClock('19:00')">19:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-secondary" @click="setClock('20:00')">20:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-secondary" @click="setClock('21:00')">21:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-primary" @click="setClock('22:00')">22:00</button>
              </div>
              <div class="form-group w-25">
                <button type="button" class="uk-button uk-button-primary" @click="setClock('23:00')">23:00</button>
              </div>
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
        clock: 0,
        details: [],
        menu: true,
        errors: [],
        clockModal: false,
        updating: false
      }
    },
    async asyncData({params,$auth,$axios,redirect}) {
      if(!$auth.loggedIn)
        redirect('/');

      const {data} = await $axios.post('api/v1/user/residence/editDetail',{id:params.id});

      return {
        id: params.id,
        details: data.details
      };
    },
    methods: {
      chooseClock(clock) {
        this.clock = clock;
        this.clockModal = true;
      },
      setClock(clock) {
        if(this.clock === 1) {
          this.details.startDeliveryTime = clock;
        }
        if(this.clock === 2) {
          this.details.endDeliveryTime = clock;
        }
        if(this.clock === 3) {
          this.details.evacuateTime = clock;
        }
        this.clockModal = false;
      },
      async setCities() {
        if(this.selectedProvince !== null)
          this.cities = this.selectedProvince.cities;
        else
          this.cities = [];
      },
      async updateDetail() {
        this.updating = true;
        this.errors = [];
        const newResidence = await this.$axios.post('api/v1/user/residence/updateDetail',this.details);
        if(newResidence.data.status === 1) {
          this.$snotify.success(newResidence.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.$router.push('/profile/residence/edit/calendar/'+this.id);
        } else if(newResidence.data.status === 422) {
          this.errors = newResidence.data.errors;
          this.$snotify.warning(newResidence.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(newResidence.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
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
