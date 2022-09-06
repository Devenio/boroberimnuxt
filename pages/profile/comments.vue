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
                <nuxt-link to="/profile/reserves">
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
                <nuxt-link to="/profile/comments" class="active">
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

        <div class="body comments-list">
          <h1 class="profile-title">
            <i class="bx bx-chat"></i>
            نظرات کاربران
          </h1>
          <div class="show-lists">
            <div class="profile-links">
              <a href="javascript:void(0)" class="active">
                <span class="name">همه نظرات</span>
                <span class="counter">{{ comments.length }}</span>
              </a>
            </div>

            <div class="comments-items">
              <div class="title-items">
                <div class="img-name">عنوان آگهی</div>
                <div class="user-info">کاربر</div>
                <div class="date">تاریخ ثبت</div>
              </div>
              <div class="item" v-for="comment in comments">
                <div class="top">
                  <div class="img-name">
                    <div class="user-info">
                      <span>{{ comment.residence }}</span>
                    </div>
                  </div>
                  <div class="user-info">
                    <span>{{ comment.user }}</span>
                  </div>
                  <div class="date">
                    <span>
                    {{ comment.date }}
                    </span>
                  </div>
                </div>
                <div class="bottom" v-html="comment.text"></div>
              </div>
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
  import { mapGetters } from 'vuex';

  export default {
    layout: 'profile',
    data() {
      return {
        status: 1,
        comments: [],
        skip: 0
      }
    },
    async asyncData({$auth,$axios,redirect}) {
      if(!$auth.loggedIn)
        redirect('/');

      const {data} = await $axios.post('api/v1/user/comments',{skip:0});

      return {
        comments: data.comments
      };
    },
    methods: {
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
        title: 'لیست اقامتگاه‌ها - '+this.settings.sitename,
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
