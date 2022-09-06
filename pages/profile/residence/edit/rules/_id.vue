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
                <li class="active">
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
              <p class="form-name">قوانین و مقررات</p>
              <p class="form-description">
                قوانین مربوط به اقامتگاه را انتخاب کنید تا مهمانان انتخاب صحیح‌تری داشته باشند.
              </p>
              <form action="">
                <no-ssr>
                  <div class="rules" v-for="rule in rules">
                    <p v-if="rule.type === 1">کهنسالان و نوزاد ها</p>
                    <p v-else-if="rule.type === 2">افراد دارای معلولیت</p>
                    <p v-else>سایر قوانین</p>
                    <div class="inner">
                      <input type="checkbox" :id="`rule`+rule.id" class="uk-checkbox" :value="rule.check" v-model="rule.check">
                      <label :for="`rule`+rule.id" v-html="rule.text"></label>
                    </div>
                  </div>
                </no-ssr>
                <no-ssr>
                  <div class="rules" v-for="rule in otherRules">
                    <p>{{ rule.title }}</p>
                    <div class="inner" v-for="detail in rule.details">
                      <input type="radio" :id="`orule`+rule.id" class="uk-radio" :value="detail.id" v-model="rule.checkId">
                      <label :for="`orule`+rule.id" v-html="detail.text"></label>
                    </div>
                  </div>
                </no-ssr>
                <div class="row submit-cancel">
                  <button class="submit" type="button" v-if="updating === false" @click="updateRules">
                    مرحله بعد
                  </button>
                  <button class="submit" type="button" v-else>
                    چند لحظه صبر کنید
                  </button>
                  <nuxt-link class="cancel" :to="`/profile/residence/edit/map/`+id">مرحله قبل</nuxt-link>
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
        menu: true,
        errors: [],
        rules: [],
        otherRules: [],
        updating: false
      }
    },
    async asyncData({params,$auth,$axios,redirect}) {
      if(!$auth.loggedIn)
        redirect('/');

      const {data} = await $axios.post('api/v1/user/residence/editRule',{id:params.id});

      return {
        id: params.id,
        rules: data.rules,
        otherRules: data.otherRules
      };
    },
    methods: {
      async updateRules() {
        var ruleIds = [];
        var otherRuleIds = [];
        for(const rule of this.rules) {
          if(rule.check)
            ruleIds.push(rule.id);
        }
        for(const otherRule of this.otherRules)
          otherRuleIds[otherRule.id] = otherRule.checkId;
        if(this.updating === false) {
          this.updating = true;
          const updateRules = await this.$axios.post('api/v1/user/residence/updateRules',{id:this.id,ruleIds:ruleIds,otherRuleIds:otherRuleIds});
          if(updateRules.data.status === 1) {
            this.$snotify.success(updateRules.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
            this.$router.push('/profile/residence/edit/crules/'+this.id);
          } else if(updateRules.data.status === 422) {
            this.errors = updateRules.data.errors;
            this.$snotify.warning(updateRules.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          } else {
            this.$snotify.error(updateRules.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          }
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
