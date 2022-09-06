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
                <li class="active">
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
              <p class="form-name">سرویس بهداشتی و حمام</p>
              <p class="form-description">
                اطلاعات مربوط به سرویس‌های بهداشتی و حمام اقامتگاه را به‌همراه امکانات هریک وارد کنید.
              </p>
              <form action="">
                <div class="row">
                  <div class="form-group uk-margin-top w-30">
                    <input type="text" id="title" v-model="newWc.title" required>
                    <span class="floating-label">عنوان</span>
                  </div>
                  <div class="form-group uk-margin-top w-30">
                    <input type="text" id="distanceWalk" v-model="newWc.position" required>
                    <span class="floating-label">موقعیت مکانی (داخل واحد، داخل حیاط و...)</span>
                  </div>
                  <div class="form-group checkbox checkbox-with-input w-30">
                    <input type="checkbox" class="uk-checkbox" id="share" v-model="newWc.share" :checked="newWc.share">
                    <label for="share">سرویس مشترک</label>
                  </div>
                </div>
                <div class="row">
                  <div class="form-group checkbox w-25" v-for="wcOption in wcOptions">
                    <input type="checkbox" class="uk-checkbox" :id="wcOption.id" @change="addWcOption(wcOption.id)">
                    <label :for="wcOption.id">{{ wcOption.title }}</label>
                  </div>
                </div>
                <div class="row submit-cancel">
                  <button type="button" class="submit" v-if="creating === false" @click="createWc">ثبت سرویس جدید</button>
                  <button type="button" class="submit" v-else>چند لحظه صبر کنید</button>
                </div>
              </form>
              <div class="show-lists">
                <hr>
                <br>
                <div class="uk-overflow-auto">
                  <p class="form-name uk-margin-medium-bottom">لیست سرویس ها</p>
                  <table class="uk-table uk-table-middle">
                    <thead>
                    <tr>
                      <th>#</th>
                      <th>عنوان</th>
                      <th>عملیات</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(wc,index) in wcs">
                      <td class="date" style="padding-right: 15px;">{{ index+1 }}</td>
                      <td class="date">{{ wc.title }}</td>
                      <td class="more-action">
                        <a href="javascript:void(0)" class="editBtn" @click="setWc(wc)">
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
              <br>
              <hr>
              <form action="">
                <div class="row">
                  <p class="form-name w-100 uk-margin-medium-bottom">امکانات بهداشتی اقامتگاه</p>
                  <div class="form-group checkbox w-30" v-for="hygieneOption in hygieneOptions">
                    <input type="checkbox" class="uk-checkbox" :id="`hygieneOption`+hygieneOption.id"
                           :checked="hygieneOption.checked" @change="changeHygineOption(hygieneOption)">
                    <label :for="`hygieneOption`+hygieneOption.id">{{ hygieneOption.title }}</label>
                  </div>
                </div>
              </form>
              <form>
                <div class="row submit-cancel">
                  <nuxt-link class="submit" :to="`/profile/residence/edit/rooms/`+id">
                    <i class="bx bx-chevron-right"></i>
                    مرحله بعد
                  </nuxt-link>
                  <nuxt-link class="cancel" :to="`/profile/residence/edit/facilityAndHygieneOptions/`+id">مرحله قبل</nuxt-link>
                </div>
              </form>
            </div>
          </div>
        </div>
        <vk-modal center id="editModalWc" class="modal-info-submit" :show.sync="editModalWc">
          <div class="title">
            <p>
              بروزرسانی سرویس بهداشتی
            </p>
            <vk-modal-close @click="editModalWc = false"></vk-modal-close>
          </div>
          <div class="text">
            <div class="row">
              <div class="form-group uk-margin-top w-30">
                <input type="text" id="utitle" v-model="editWc.title" required>
                <span class="floating-label">عنوان</span>
              </div>
              <div class="form-group uk-margin-top w-30">
                <input type="text" id="uposition" v-model="editWc.position" required>
                <span class="floating-label">موقعیت</span>
              </div>
              <div class="checkbox checkbox-with-input w-30" style="margin-top: 12px">
                <div>
                  <input type="checkbox" class="uk-checkbox" id="ushare" v-model="editWc.share" :checked="editWc.share">
                  <label for="ushare">سرویس مشترک</label>
                </div>
              </div>
            </div>
            <div class="row">
              <div class="checkbox checkbox-with-input w-25" v-for="wcOption in editWc.wcOptions">
                <div>
                  <input type="checkbox" class="uk-checkbox" :id="`u`+wcOption.id" v-model="wcOption.checked" :checked="wcOption.checked">
                  <label :for="`u`+wcOption.id">{{ wcOption.title }}</label>
                </div>
              </div>
            </div>
            <div class="submit-cancel">
              <button type="button" class="submit" v-if="updating === false" @click="updateWc">
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
        menu: true,
        wcs: [],
        newWc: {
          title: null,
          position: null,
          share: false,
          wcOptions: []
        },
        editWc: {
          title: null,
          position: null,
          share: false,
          wcOptions: []
        },
        wcOptions: [],
        hygieneOptions: [],
        editModalWc: false,
        creating: false,
        updating: false
      }
    },
    async asyncData({params,$auth,$axios,redirect}) {
      if(!$auth.loggedIn)
        redirect('/');

      const {data} = await $axios.post('api/v1/user/residence/editWcs',{id:params.id});

      return {
        id:params.id,
        wcs: data.wcs,
        wcOptions: data.wcOptions,
        hygieneOptions: data.hygieneOptions
      };
    },
    methods: {
      addWcOption(id) {
        if(!this.newWc.wcOptions.includes(id))
          this.newWc.wcOptions.push(id);
        else
          this.newWc.wcOptions.splice(this.newWc.wcOptions.indexOf(id), 1);
      },
      async createWc() {
        this.creating = true;
        this.errors = [];
        const saveWc = await this.$axios.post('api/v1/user/residence/saveWc',
          {id: this.id,title: this.newWc.title, position: this.newWc.position, share: this.newWc.share, wcOptions: this.newWc.wcOptions});
        if(saveWc.data.status === 1) {
          this.$snotify.success(saveWc.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          const data = await this.$axios.post('api/v1/user/residence/editWcs',{id:this.id});
          this.wcs = data.data.wcs;
          this.wcOptions = data.data.wcOptions;
          this.newWc = {
            title: null,
            position: null,
            share: false,
            wcOptions: []
          };
        } else if(saveWc.data.status === 422) {
          this.errors = saveWc.data.errors;
          this.$snotify.warning(saveWc.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(saveWc.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.creating = false;
      },
      async updateWc() {
        this.updating = true;
        this.errors = [];
        const updateWc = await this.$axios.post('api/v1/user/residence/updateWc',
          {id: this.editWc.id,title: this.editWc.title, position: this.editWc.position, share: this.editWc.share, wcOptions: this.editWc.wcOptions});
        if(updateWc.data.status === 1) {
          this.$snotify.success(updateWc.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.editModalWc = false;
          const data = await this.$axios.post('api/v1/user/residence/editWcs',{id:this.id});
          this.wcs = data.data.wcs;
          this.wcOptions = data.data.wcOptions;
          this.editWc = {
            title: null,
            position: null,
            share: false,
            wcOptions: []
          };
        } else if(updateWc.data.status === 422) {
          this.errors = updateWc.data.errors;
          this.$snotify.warning(updateWc.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(updateWc.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.updating = false;
      },
      async changeHygineOption(hygieneOption) {
        let ar = [];
        hygieneOption.checked = !hygieneOption.checked;
        this.hygieneOptions.forEach(element => {
          if(element.checked !== false)
            ar.push(element.id);
        });
        await this.$axios.post('api/v1/user/residence/updateHygineOptions/'+this.id,{options:ar});
      },
      setWc(wc) {
        this.editWc = wc;
        this.editModalWc = true;
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
