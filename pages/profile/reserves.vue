<template>
  <div>
    <main ref="main" class="main" :class="[vuexMenuState ? 'blur' : '',vuexLogregMobileState ? 'blur' : '']" @click="closeMenu">
      <div class="user-profile-title">
        <div class="mobile-icon">
          <a href="javascript:void(0)" @click="openProfileMenu">
            <img src="/images/burger.svg" alt="">
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

      <section class="profile">
        <aside class="sidebar" :class="[vuexProfileMenuState ? 'active' : '']">
          <div class="top">
            <ul>
              <li>
                <nuxt-link to="/profile">
                  <i class="bx bx-chart"></i>
                  داشبورد
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile/reserves" class="active">
                  <i class="bx bx-git-pull-request"></i>
                  لیست رزروها
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile/residences">
                  <i class="bx bx-building-house"></i>
                  مدیریت اقامتگاه‌ها
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile/comments">
                  <i class="bx bx-chat"></i>
                  نظرات کاربران
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile/complains">
                  <i class="bx bx-chat"></i>
                  شکایات کاربران
                </nuxt-link>
              </li>
            </ul>
          </div>
          <div class="bottom">
            <ul>
              <li>
                <nuxt-link to="/checkOut">
                  <i class="bx bx-shopping-bag"></i>
                  سفرهای من
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile?panel=1">
                  <i class="bx bx-user-check"></i>
                  ویرایش مشخصات
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile/residence/new">
                  <i class="bx bx-star"></i>
                  ثبت اقامتگاه
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile?panel=2">
                  <i class="bx bx-heart"></i>
                  علاقه مندی‌ها
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile?panel=10">
                  <i class="bx bx-heart"></i>
                  بازاریابی
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile?panel=4">
                  <i class="bx bx-wallet"></i>
                  کیف پول و تراکنش‌ها
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile?panel=5">
                  <i class="bx bx-credit-card"></i>
                  تسویه حساب
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile?panel=6">
                  <i class="bx bx-error"></i>
                  شکایت‌ها
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile?panel=7">
                  <i class="bx bx-headphone"></i>
                  پشتیبانی
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/profile?panel=9">
                  <i class="bx bx-credit-card"></i>
                  کارت‌های بانکی
                </nuxt-link>
              </li>
            </ul>
          </div>
          <div class="logout">
            <a href="javascript:void(0)" @click="logout">
              <i class="bx bx-log-out-circle"></i>
              خروج از حساب کاربری
            </a>
          </div>
        </aside>

        <div class="body reserve-list">
          <h1 class="profile-title">
            <i class="bx bx-git-pull-request"></i>
            لیست رزروها
          </h1>
          <p class="profile-description">
            وضعیت رزروهای جاری و گذشته را مدیریت کنید.
          </p>
          <div class="show-lists">
            <div class="profile-links">
              <a href="javascript:void(0)" v-bind:class="(status === 3)?'active':''" @click="changeStatus(3)">
                <span class="name">تایید شده</span>
                <span class="counter">{{ accepted }}</span>
              </a>
              <a href="javascript:void(0)" v-bind:class="(status === 1)?'active':''" @click="changeStatus(1)">
                <span class="name">در انتظار تأیید</span>
                <span class="counter">{{ pending }}</span>
              </a>
              <a href="javascript:void(0)" v-bind:class="(status === 2)?'active':''" @click="changeStatus(2)">
                <span class="name">پرداخت شده</span>
                <span class="counter">{{ payed }}</span>
              </a>
              <a href="javascript:void(0)" v-bind:class="(status === 5)?'active':''" @click="changeStatus(5)">
                <span class="name">کنسل شده</span>
                <span class="counter">{{ canceled }}</span>
              </a>
              <a href="javascript:void(0)" v-bind:class="(status === 7)?'active':''" @click="changeStatus(7)">
                <span class="name">رد شده</span>
                <span class="counter">{{ rejected }}</span>
              </a>
              <a href="javascript:void(0)" v-bind:class="(status === 11)?'active':''" @click="changeStatus(11)">
                <span class="name">تمام شده</span>
                <span class="counter">{{ ended }}</span>
              </a>
              <a href="javascript:void(0)" v-bind:class="(status === 10)?'active':''" @click="changeStatus(10)">
                <span class="name">شکایت مهمان</span>
                <span class="counter">{{ complained }}</span>
              </a>
            </div>
            <div class="reserve-items">
              <template v-for="reserve in reserves">
                <div class="items">
                  <div class="top">
                    <div class="right">
                      <div class="img-name">
                        <div class="img">
                          <img :src="reserve.image" alt="">
                        </div>
                        <div class="name">
                          <p>{{ reserve.title }}</p> <span class="code">شماره : {{ reserve.id }}</span>
                        </div>
                      </div>
                      <div class="price">
                        <span>جمع‌:‌</span><span>{{ number_3_3(reserve.amount) }}</span>
                      </div>
                      <div class="confirmed">
                        <span class="uk-text-secondary" style="background: rgb(144, 245, 128);" v-if="reserve.status === 2">
                          <i class="bx bxs-circle uk-text-primary"></i>
                          پرداخت‌شده
                        </span>
                        <span class="uk-text-secondary" style="background: rgb(253, 253, 92);" v-if="reserve.status === 1 || reserve.status === 3 || reserve.status === 4">
                          <i class="bx bxs-circle uk-text-danger"></i>
                          پرداخت‌نشده
                        </span>
                        <span class="uk-text-secondary" style="background: rgb(243, 160, 96);" v-if="reserve.status === 5 || reserve.status === 6">
                          <i class="bx bxs-circle uk-text-secondary"></i>
                          کنسل‌شده
                        </span>
                        <span class="uk-text-secondary" style="background: rgb(243, 160, 96);" v-if="reserve.status === 7">
                          <i class="bx bxs-circle uk-text-secondary"></i>
                          ردشده
                        </span>
                        <span style="background-color: #7482ff; color: #ffffff;" v-if="reserve.status === 8 || reserve.status === 9">
                          <i class="bx bxs-circle"></i>
                          شروع‌اقامت
                        </span>
                        <span style="background-color: #7482ff; color: #ffffff;" v-if="reserve.status === 11 || reserve.status === 12">
                          <i class="bx bxs-circle"></i>
                          پایان‌اقامت
                        </span>
                        <span class="uk-text-secondary" style="background: rgb(253, 253, 92);" v-if="reserve.status === 10">
                          <i class="bx bxs-circle uk-text-danger"></i>
                          رسیدگی به شکایت
                        </span>
                      </div>
                    </div>
                    <div class="left" v-if="requesting === false">
                      <a href="javascript:void(0)" class="confirm" @click="accept(reserve.id)" v-if="reserve.ownerStatus === 1">
                        <span class="icon">
                            <i class="bx bx-check"></i>
                        </span>
                        <span class="name">تایید</span>
                      </a>
                      <a href="javascript:void(0)" class="confirm" @click="changeReserveStatus(reserve.id,8)" v-if="reserve.status === 2 && reserve.ownerStatus === 3 && reserve.started === 1">
                        <span class="icon">
                          <i class="bx bx-check"></i>
                        </span>
                        <span class="name">تحویل‌اقامتگاه</span>
                      </a>
                      <a href="javascript:void(0)" class="confirm" @click="changeReserveStatus(reserve.id,12)" v-if="reserve.status === 8 && reserve.ownerStatus === 3">
                        <span class="icon">
                          <i class="bx bx-check"></i>
                        </span>
                        <span class="name">تخلیه‌اقامتگاه</span>
                      </a>
                      <a href="javascript:void(0)" class="confirm">
                        <span class="icon">
                            <i class="bx bx-chat"></i>
                        </span>
                        <span class="name">گفتگو</span>
                      </a>
                      <template v-if="reserve.ownerStatus !== 2 && reserve.ended === 2">
                        <a href="javascript:void(0)" class="cancel" @click="reject(reserve.id)" v-if="reserve.status !== 1 || reserve.status !== 2 || reserve.status !== 3
                      || reserve.status !== 4 || reserve.status !== 8 || reserve.status !== 9">
                          <span class="icon">
                            <i class="bx bx-x"></i>
                          </span>
                          <span class="name">ردکردن</span>
                        </a>
                      </template>
                    </div>
                    <div class="left" v-else>
                      <a href="javascript:void(0)" class="confirm" v-if="reserve.ownerStatus === 1">
                        <span class="icon">
                            <i class="bx bx-check"></i>
                        </span>
                        <span class="name">تایید</span>
                      </a>
                      <a href="javascript:void(0)" class="confirm" v-if="reserve.status === 2 && reserve.ownerStatus === 3">
                        <span class="icon">
                          <i class="bx bx-check"></i>
                        </span>
                        <span class="name">تحویل‌اقامتگاه</span>
                      </a>
                      <a href="javascript:void(0)" class="confirm" v-if="reserve.status === 8 && reserve.ownerStatus === 3">
                        <span class="icon">
                          <i class="bx bx-check"></i>
                        </span>
                        <span class="name">تخلیه‌اقامتگاه</span>
                      </a>
                      <a href="javascript:void(0)" class="confirm">
                        <span class="icon">
                            <i class="bx bx-chat"></i>
                        </span>
                        <span class="name">گفتگو</span>
                      </a>
                      <template v-if="reserve.ownerStatus !== 2 && reserve.ended === 2">
                        <a href="javascript:void(0)" class="cancel" v-if="reserve.status !== 1 || reserve.status !== 2 || reserve.status !== 3
                      || reserve.status !== 4 || reserve.status !== 8 || reserve.status !== 9">
                        <span class="icon">
                            <i class="bx bx-x"></i>
                        </span>
                          <span class="name">ردکردن</span>
                        </a>
                      </template>
                    </div>
                  </div>
                  <div class="middle" v-if="reserve.ownerStatus === 3 && reserve.status !== 1 && reserve.status !== 3 && reserve.status !== 4">
                    <img :src="reserve.userImage" alt="">
                    <span class="name">{{ reserve.user }}</span>
                    <span class="melli_code">کد ملی : {{ reserve.codeMelli }}</span>
                    <span class="phone">شماره تماس : {{ reserve.mobile }}</span>
                  </div>
                  <div class="bottom">
                    <div class="right">
                      <div class="date"> <span>تاریخ ورود : </span> <p>{{ reserve.startDate }}</p> </div>
                      <div class="date"> <span>تاریخ خروج : </span> <p>{{ reserve.endDate }}</p> </div>
                      <div class="date"> <span>تعداد مهمان : </span> <p>{{ reserve.person }} نفر</p> </div>
                    </div>
                    <div class="left" v-if="reserve.ownerStatus === 1 && reserve.status !== 5 && reserve.status !== 6
                    && reserve.status !== 7 && reserve.status !== 10">
                      <Counter :hour="reserve.hourRemaining" :minute="reserve.minuteRemaining" />
                    </div>
                  </div>
                </div>
                <hr>
              </template>
            </div>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<style scoped>
  * {
    box-sizing: inherit !important;
  }
</style>

<script>
  import Counter from '~/components/Counter';
  import { mapGetters } from 'vuex';

  export default {
    layout: 'profile',
    components: {
      Counter
    },
    data() {
      return {
        status: 3,
        all: 0,
        pending: 0,
        accepted: 0,
        payed: 0,
        canceled: 0,
        rejected: 0,
        ended: 0,
        complained: 0,
        reserves: [],
        errors: [],
        skip: 0,
        requesting: false
      }
    },
    async asyncData({$auth,$axios,redirect}) {
      if(!$auth.loggedIn)
        redirect('/');

      const {data} = await $axios.post('api/v1/user/reserves',{skip:0});

      return {
        pending: data.pendingReserves,
        accepted: data.acceptedReserves,
        payed: data.payedReserves,
        canceled: data.canceledReserves,
        rejected: data.rejectedReserves,
        ended: data.endedReserves,
        complained: data.complainedReserves,
        reserves: data.reserves
      };
    },
    methods: {
      async changeStatus(number) {
        this.status = number;
        const reserves = await this.$axios.post('api/v1/user/reserves',{skip:this.skip,status:this.status});
        this.pending = reserves.data.pendingReserves;
        this.accepted = reserves.data.acceptedReserves;
        this.payed = reserves.data.payedReserves;
        this.canceled = reserves.data.canceledReserves;
        this.rejected = reserves.data.rejectedReserves;
        this.ended = reserves.data.endedReserves;
        this.complained = reserves.data.complainedReserves;
        this.reserves = reserves.data.reserves;
      },
      async accept(id) {
        if(!confirm("از قبول کردن درخواست شماره "+id+" مطمئن هستید ؟ "))
          return;
        this.requesting = true;
        let acceptReserve = await this.$axios.post('api/v1/user/acceptReserve',{id:id});
        if(acceptReserve.data.status === 1) {
          this.$snotify.success(acceptReserve.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          const reserves = await this.$axios.post('api/v1/user/reserves',{skip:this.skip,status:3});
          this.pending = reserves.data.pendingReserves;
          this.accepted = reserves.data.acceptedReserves;
          this.payed = reserves.data.payedReserves;
          this.canceled = reserves.data.canceledReserves;
          this.rejected = reserves.data.rejectedReserves;
          this.ended = reserves.data.endedReserves;
          this.complained = reserves.data.complainedReserves;
          this.reserves = reserves.data.reserves;
          this.status = 3;
        } else if(acceptReserve.data.status === 422) {
          this.errors = acceptReserve.data.errors;
          this.$snotify.warning(acceptReserve.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(acceptReserve.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.requesting = false;
      },
      async reject(id) {
        if(!confirm("از رد کردن درخواست شماره "+id+" مطمئن هستید ؟ "))
          return;
        this.requesting = true;
        let acceptReserve = await this.$axios.post('api/v1/user/rejectReserve',{id:id});
        if(acceptReserve.data.status === 1) {
          this.$snotify.success(acceptReserve.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          const reserves = await this.$axios.post('api/v1/user/reserves',{skip:this.skip,status:7});
          this.pending = reserves.data.pendingReserves;
          this.accepted = reserves.data.acceptedReserves;
          this.payed = reserves.data.payedReserves;
          this.canceled = reserves.data.canceledReserves;
          this.rejected = reserves.data.rejectedReserves;
          this.ended = reserves.data.endedReserves;
          this.complained = reserves.data.complainedReserves;
          this.reserves = reserves.data.reserves;
          this.status = 7;
        } else if(acceptReserve.data.status === 422) {
          this.errors = acceptReserve.data.errors;
          this.$snotify.warning(acceptReserve.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(acceptReserve.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.requesting = false;
      },
      async changeReserveStatus(id,newStatus) {
        this.requesting = true;
        this.errors = [];
        let changeReserveStatus = await this.$axios.post('api/v1/user/changeReserveStatus',{id:id,'newStatus':newStatus});
        if(changeReserveStatus.data.status === 1) {
          this.$snotify.success(changeReserveStatus.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          const reserves = await this.$axios.post('api/v1/user/reserves',{skip:this.skip,status:this.status});
          this.pending = reserves.data.pendingReserves;
          this.accepted = reserves.data.acceptedReserves;
          this.payed = reserves.data.payedReserves;
          this.canceled = reserves.data.canceledReserves;
          this.rejected = reserves.data.rejectedReserves;
          this.ended = reserves.data.endedReserves;
          this.complained = reserves.data.complainedReserves;
          this.reserves = reserves.data.reserves;
        } else if(changeReserveStatus.data.status === 422) {
          this.$snotify.warning(changeReserveStatus.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(changeReserveStatus.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.requesting = false;
      },
      number_3_3(num, sep) {
        const number = typeof num === 'number' ? num.toString() : num;
        const separator = typeof sep === 'undefined' ? ',' : sep;
        if(typeof number !== 'undefined')
          return number.replace(/(\d)(?=(\d\d\d)+(?!\d))/g, '$1' + separator);
      },
      openProfileMenu() {
        if(this.vuexProfileMenuState === true) {
          this.$store.dispatch('setVuexProfileMenuState',false);
        } else {
          this.$store.dispatch('setVuexProfileMenuState',true);
        }
      },
      closeProfileMenu() {
        const dashboard = this.$refs.dashboard;
        if (dashboard.classList.contains('blur'))
          this.$store.dispatch('setVuexProfileMenuState',false);
      },
      closeMenu() {
        const main = this.$refs.main;
        if (main.classList.contains('blur')) {
          this.$store.dispatch('setVuexMenuState',false);
          this.$store.dispatch('setVuexLogregMobileState',false);
        }
      },
      logout() {
        this.$auth.logout();
        this.$router.push('/');
      }
    },
    computed: {
      vuexProfileMenuState() {
        return this.$store.getters.vuexProfileMenuState
      },
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
        title: 'لیست رزرو‌ها - '+this.settings.sitename,
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
