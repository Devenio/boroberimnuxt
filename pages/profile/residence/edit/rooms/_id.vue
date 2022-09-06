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
                <li class="active">
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
              <p class="form-name">اتاق‌ها</p>
              <p class="form-description">
                اطلاعات مربوط به اتاق‌های مختلف اقامتگاه را همراه با امکانات هر کدام از آن‌ها وارد کنید. توجه که اتاق نشیمن یا پذیرایی را در لیست این اتاق‌ها قرار ندهید.
              </p>
              <form action="">
                <div class="row">
                  <div class="form-group w-50">
                    <input type="text" id="title" v-model="newRoom.title" required>
                    <span class="floating-label">عنوان اتاق (مثال: اتاق خواب یک ...)</span>
                    <span v-if="errors.title" class="error">{{ errors.title[0] }}</span>
                  </div>
                  <div class="form-group w-50">
                    <input type="text" id="floor" v-model="newRoom.floor" required>
                    <span class="floating-label">طبقه اتاق (مثال: همکف، اول و ...)</span>
                    <span v-if="errors.floor" class="error">{{ errors.floor[0] }}</span>
                  </div>
                </div>
                <div class="row">
                  <div class="form-group checkbox w-25">
                    <input type="checkbox" class="uk-checkbox" id="onebed" v-model="newRoom.onebed" :checked="newRoom.onebed">
                    <label for="onebed">تخت خواب 1 نفره</label>
                  </div>
                  <div class="form-group checkbox w-25">
                    <input type="checkbox" class="uk-checkbox" id="twobed" v-model="newRoom.twobed" :checked="newRoom.twobed">
                    <label for="twobed">تخت خواب 2 نفره</label>
                  </div>
                  <div class="form-group checkbox w-25">
                    <input type="checkbox" class="uk-checkbox" id="sofabed" v-model="newRoom.sofabed" :checked="newRoom.sofabed">
                    <label for="sofabed">مبل تخت خواب شو</label>
                  </div>
                  <div class="form-group checkbox w-25">
                    <input type="checkbox" class="uk-checkbox" id="master_room" v-model="newRoom.master_room" :checked="newRoom.master_room">
                    <label for="master_room">اتاق مستر</label>
                  </div>
                </div>
                <div class="row">
                  <div class="form-group w-30" v-if="newRoom.onebed">
                    <label for="onebedCount">تعداد تخت 1 نفره</label>
                    <input type="text" id="onebedCount" v-model="newRoom.onebed_count">
                  </div>
                  <div class="form-group w-30" v-if="newRoom.twobed">
                    <label for="twobedCount">تعداد تخت 2 نفره</label>
                    <input type="text" id="twobedCount" v-model="newRoom.twobed_count">
                  </div>
                  <div class="form-group w-30" v-if="newRoom.sofabed">
                    <label for="sofabedCount">تعداد مبل تخت‌خواب‌شو</label>
                    <input type="text" id="sofabedCount" v-model="newRoom.sofabed_count">
                  </div>
                </div>
                <hr>
                <div class="row">
                  <p class="form-name uk-margin-medium-bottom w-100">امکانات گرمایشی اتاق</p>
                  <div class="form-group checkbox w-25" v-for="heater in heaters">
                    <input type="checkbox" class="uk-checkbox" :id="`h`+heater.id" @change="addHeater(heater.id)">
                    <label :for="`h`+heater.id">{{ heater.title }}</label>
                  </div>
                </div>
                <div class="row">
                  <p class="form-name uk-margin-medium-bottom w-100">امکانات سرمایشی اتاق</p>
                  <div class="form-group checkbox w-25" v-for="cooler in coolers">
                    <input type="checkbox" class="uk-checkbox" :id="`c`+cooler.id" @change="addCooler(cooler.id)">
                    <label :for="`c`+cooler.id">{{ cooler.title }}</label>
                  </div>
                </div>
                <div class="row submit-cancel">
                  <button type="button" class="submit" v-if="creating === false" @click="createRoom">ثبت اتاق جدید</button>
                  <button type="button" class="submit" v-else>چند لحظه صبر کنید</button>
                </div>
              </form>
              <div class="show-lists">
                <hr>
                <p class="form-name w-100 uk-margin-medium-bottom">لیست اتاق‌ها</p>
                <div class="uk-overflow-auto">
                  <table class="uk-table uk-table-middle ">
                    <thead>
                    <tr>
                      <th>#</th>
                      <th>عنوان</th>
                      <th>تخت یک نفره</th>
                      <th>تخت دو نفره</th>
                      <th>مبل تخت‌خواب‌شو</th>
                      <th>اتاق مستر</th>
                      <th>عملیات</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(room,index) in rooms">
                      <td class="date" style="padding-right: 15px;">{{ index+1 }}</td>
                      <td class="date">{{ room.title }}</td>
                      <td class="date">
                        <span v-if="room.onebed === 0">
                          ندارد
                        </span>
                        <span v-else>
                          {{ room.onebed_count }} عدد
                        </span>
                      </td>
                      <td class="date">
                        <span v-if="room.twobed === 0">
                          ندارد
                        </span>
                        <span v-else>
                          {{ room.twobed_count }} عدد
                        </span>
                      </td>
                      <td class="date">
                        <span v-if="room.sofabed === 0">
                          ندارد
                        </span>
                        <span v-else>
                          {{ room.sofabed_count }} عدد
                        </span>
                      </td>
                      <td class="date">
                        <span v-if="room.master_room === 1">
                          می‌باشد
                        </span>
                        <span v-else>
                          نمی‌باشد
                        </span>
                      </td>
                      <td class="more-action">
                        <a href="javascript:void(0)" class="editBtn" @click="setRoom(room)">
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
                  <nuxt-link class="submit" :to="`/profile/residence/edit/ownership/`+id">مرحله بعد</nuxt-link>
                  <nuxt-link class="cancel" :to="`/profile/residence/edit/wcs/`+id">مرحله قبل</nuxt-link>
                </div>
              </form>
            </div>
          </div>
        </div>
        <vk-modal center id="editModalRoom" class="modal-info-submit" :show.sync="editModalRoom">
          <div class="title">
            <p>
              بروزرسانی اتاق
            </p>
            <vk-modal-close @click="editModalRoom = false"></vk-modal-close>
          </div>
          <div class="text">
            <div class="row">
              <div class="form-group w-50">
                <input type="text" id="utitle" v-model="editRoom.title" required>
                <span class="floating-label">عنوان</span>
              </div>
              <div class="form-group w-50">
                <input type="text" id="ufloor" v-model="editRoom.floor" required>
                <span class="floating-label">طبقه</span>
              </div>
            </div>
            <div class="row">
              <div class="checkbox checkbox-with-input w-50">
                <div>
                  <input type="checkbox" class="uk-checkbox" id="uonebed" v-model="editRoom.onebed" :checked="editRoom.onebed">
                  <label for="uonebed">تخت خواب 1 نفره</label>
                </div>
              </div>
              <div class="checkbox checkbox-with-input w-50">
                <div>
                  <input type="checkbox" class="uk-checkbox" id="utwobed" v-model="editRoom.twobed" :checked="editRoom.twobed">
                  <label for="utwobed">تخت خواب 2 نفره</label>
                </div>
              </div>
              <div class="checkbox checkbox-with-input w-50">
                <div>
                  <input type="checkbox" class="uk-checkbox" id="usofabed" v-model="editRoom.sofabed" :checked="editRoom.sofabed">
                  <label for="usofabed">مبل تخت‌خواب‌شو</label>
                </div>
              </div>
              <div class="checkbox checkbox-with-input w-50">
                <div>
                  <input type="checkbox" class="uk-checkbox" id="umaster_room" v-model="editRoom.master_room" :checked="editRoom.master_room">
                  <label for="umaster_room">اتاق مستر</label>
                </div>
              </div>
            </div>
            <div class="row">
              <div class="form-group w-100" v-if="editRoom.onebed">
                <input type="text" id="onebed_count" v-model="editRoom.onebed_count" required>
                <span class="floating-label">تعداد تخت یک نفره</span>
              </div>
              <div class="form-group w-100" v-if="editRoom.twobed">
                <input type="text" id="twobed_count" v-model="editRoom.twobed_count" required>
                <span class="floating-label">تعداد تخت دو نفره</span>
              </div>
              <div class="form-group w-100" v-if="editRoom.sofabed">
                <input type="text" id="sofabed_count" v-model="editRoom.sofabed_count" required>
                <span class="floating-label">تعداد مبل تخت‌خواب‌شو</span>
              </div>
            </div>
            <div class="row">
              <div class="checkbox checkbox-with-input w-25" v-for="heater in editRoom.heaters">
                <p class="form-name uk-margin-medium-bottom w-100">امکانات گرمایشی اتاق</p>
                <div>
                  <input type="checkbox" class="uk-checkbox" :id="`uh`+heater.id" v-model="heater.checked" :checked="heater.checked">
                  <label :for="`uh`+heater.id">{{ heater.title }}</label>
                </div>
              </div>
            </div>
            <div class="row">
              <div class="checkbox checkbox-with-input w-25" v-for="cooler in editRoom.coolers">
                <p class="form-name uk-margin-medium-bottom w-100">امکانات سرمایشی اتاق</p>
                <div>
                  <input type="checkbox" class="uk-checkbox" :id="`uc`+cooler.id" v-model="cooler.checked" :checked="cooler.checked">
                  <label :for="`uc`+cooler.id">{{ cooler.title }}</label>
                </div>
              </div>
            </div>
            <div class="submit-cancel">
              <button type="button" class="submit" v-if="updating === false" @click="updateRoom">
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
        rooms: [],
        errors: [],
        newRoom: {
          title: null,
          floor: null,
          onebed: false,
          twobed: false,
          sofabed: false,
          onebed_count: 0,
          twobed_count: 0,
          sofabed_count: 0,
          master_room: false,
          heaters: [],
          coolers: []
        },
        editRoom: {
          title: null,
          floor: null,
          onebed: false,
          twobed: false,
          sofabed: false,
          onebed_count: 0,
          twobed_count: 0,
          sofabed_count: 0,
          master_room: false,
          heaters: [],
          coolers: []
        },
        heaters: [],
        coolers: [],
        editModalRoom: false,
        creating: false,
        updating: false
      }
    },
    async asyncData({params,$auth,$axios,redirect}) {
      if(!$auth.loggedIn)
        redirect('/');

      const {data} = await $axios.post('api/v1/user/residence/editRooms',{id:params.id});

      return {
        id:params.id,
        rooms: data.rooms,
        heaters: data.heaters,
        coolers: data.coolers
      };
    },
    methods: {
      addHeater(id) {
        if(!this.newRoom.heaters.includes(id))
          this.newRoom.heaters.push(id);
        else
          this.newRoom.heaters.splice(this.newRoom.heaters.indexOf(id), 1);
      },
      addCooler(id) {
        if(!this.newRoom.coolers.includes(id))
          this.newRoom.coolers.push(id);
        else
          this.newRoom.coolers.splice(this.newRoom.coolers.indexOf(id), 1);
      },
      async createRoom() {
        this.creating = true;
        this.errors = [];
        const saveRoom = await this.$axios.post('api/v1/user/residence/saveRoom',
          {id: this.id,title: this.newRoom.title, floor: this.newRoom.floor,
            onebed: this.newRoom.onebed, twobed: this.newRoom.twobed, sofabed: this.newRoom.sofabed,
            onebed_count: this.newRoom.onebed_count, twobed_count: this.newRoom.twobed_count, sofabed_count: this.newRoom.sofabed_count,
            master_room: this.newRoom.master_room,
            heaters: this.newRoom.heaters, coolers: this.newRoom.coolers});
        if(saveRoom.data.status === 1) {
          this.$snotify.success(saveRoom.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.$router.go();
        } else if(saveRoom.data.status === 422) {
          this.errors = saveRoom.data.errors;
          this.$snotify.warning(saveRoom.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(saveRoom.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.creating = false;
      },
      async updateRoom() {
        this.updating = true;
        this.errors = [];
        const updateRoom = await this.$axios.post('api/v1/user/residence/updateRoom',
          {id: this.editRoom.id,title: this.editRoom.title, floor: this.editRoom.floor,
            onebed: this.editRoom.onebed, twobed: this.editRoom.twobed, sofabed: this.editRoom.sofabed, master_room: this.editRoom.master_room,
            onebed_count: this.editRoom.onebed_count, twobed_count: this.editRoom.twobed_count, sofabed_count: this.editRoom.sofabed_count,
            heaters: this.editRoom.heaters, coolers: this.editRoom.coolers});
        if(updateRoom.data.status === 1) {
          this.$snotify.success(updateRoom.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.editModalRoom = false;
          this.$router.go();
        } else if(updateRoom.data.status === 422) {
          this.errors = updateRoom.data.errors;
          this.$snotify.warning(updateRoom.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(updateRoom.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.updating = false;
      },
      setRoom(room) {
        this.editRoom = room;
        this.editModalRoom = true;
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
