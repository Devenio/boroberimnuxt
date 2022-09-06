<template>
  <div>
    <main :class="[vuexMenuState ? 'blur' : '',vuexLogregMobileState ? 'blur' : '']" @click="closeMenu" class="main" ref="main">
      <section class="host-profile">
        <aside>
          <no-ssr>
            <div uk-sticky="offset: 100; bottom: true">
              <div class="reserve-request">
                <div class="host">
                  <img :src="imageUser" :alt="name">
                  <p>{{ name }}</p>
                  <span>
                  <i class="bx bx-calendar"></i>
                  عضویت از {{ registerDate }}
              </span>
                </div>
                <div class="rate-text">
                  <div class="rate">
                    <span class="icon"><i class="bx bxs-star"></i></span>
                    <span>{{ totalScore }}</span>
                  </div>
                  <div class="text">
                    <p class="blue">
                      <i class="bx bx-check-shield"></i>
                      میزبان تایید شده
                    </p>
                  </div>
                </div>
                <div class="answer-info">
                  <p>
                    <span class="first">زمان پاسخ‌گویی : </span>
                    <span class="second" v-if="totalResponseTime > 60">
                      بیش از {{ totalResponseTime/60 }} ساعت
                    </span>
                    <span class="second" v-else>
                      {{ totalResponseTime }} دقیقه
                    </span>
                  </p>
                  <p>
                    <span class="first">میزان پاسخ‌گویی : </span>
                    <span class="second">{{ totalAcceptPercent }} درصد</span>
                  </p>
                </div>
              </div>
            </div>
          </no-ssr>
        </aside>

        <div class="profile-body">
          <section class="economic-slider uk-slider">
            <div class="uk-position-relative">
              <div class="title">
                <div class="name">
                  <h3>اقامتگاه‌ها</h3>
                </div>
              </div>
              <div class="uk-slider-container">
                <ul class="uk-slider-items uk-grid">
                  <HostProfileResidenceLi :key="residence.id" :residence="residence" v-for="residence in residences"/>
                </ul>
              </div>
            </div>
          </section>

          <div class="comments">
            <div class="title">
              <i class="bx bx-comment-detail"></i>
              <h3>دیدگاه کاربران</h3>
            </div>
            <div class="feedback">
              <div class="until-now">
                <span>
                    <i class="bx bxs-star"></i>
                </span>
                <p>تاکنون {{ commentsCount }} نظر ثبت شده است</p>
              </div>
              <div class="rate-parts">
                <div class="parts">
                  <ul>
                    <li>
                      <i class="bx bxs-circle"></i>
                      <p class="name">دسترسی</p>
                      <p class="rate">{{ totalScore1 }}</p>
                    </li>
                    <li>
                      <i class="bx bxs-circle"></i>
                      <p class="name">تطابق با آگهی در سایت</p>
                      <p class="rate">{{ totalScore2 }}</p>
                    </li>
                    <li>
                      <i class="bx bxs-circle"></i>
                      <p class="name">نظافت</p>
                      <p class="rate">{{ totalScore3 }}</p>
                    </li>
                    <li>
                      <i class="bx bxs-circle"></i>
                      <p class="name">همسایگی</p>
                      <p class="rate">{{ totalScore4 }}</p>
                    </li>
                    <li>
                      <i class="bx bxs-circle"></i>
                      <p class="name">نسبت ارزش به قیمت</p>
                      <p class="rate">{{ totalScore5 }}</p>
                    </li>
                    <li>
                      <i class="bx bxs-circle"></i>
                      <p class="name">کیفیت میزبانی</p>
                      <p class="rate">{{ totalScore6 }}</p>
                    </li>
                  </ul>
                </div>
                <div class="total-rate">
                  <p>{{ totalScore }}</p>
                </div>
              </div>
            </div>
            <div class="totla-comments">
              <div class="each-comment" v-for="comment in comments">
                <div class="user-date">
                  <div class="user">
                    <div class="img">
                      <img :alt="comment.user" :src="comment.image">
                    </div>
                    <div class="name">
                      <p>{{ comment.user }}</p>
                      <div>
                          <span>
                              <i class="bx bxs-star"></i>
                          </span>
                        <span>{{ comment.score }}</span>
                      </div>
                    </div>
                  </div>
                  <p class="date">{{ comment.date }}</p>
                </div>
                <div class="pos-neg">
                  <div class="positive w-50" v-if="comment.pros.length > 0">
                    <p>نکات قوت : </p>
                    <div class="items">
                      <ul>
                        <li v-for="pro in comment.pros">
                          <i class="bx bxs-circle"></i>
                          {{ pro }}
                        </li>
                      </ul>
                    </div>
                  </div>
                  <div class="negative w-50" v-if="comment.cons.length > 0">
                    <p>نکات ضعف : </p>
                    <div class="items">
                      <ul>
                        <li v-for="con in comment.cons">
                          <i class="bx bxs-circle"></i>
                          {{ con }}
                        </li>
                      </ul>
                    </div>
                  </div>
                </div>
                <div class="text">
                  <p>{{ comment.text }}</p>
                  <template v-if="comment.myComment === 1">
                    <br>
                    <a @click="deleteComment" href="javascript:void(0)" style="color: #ff0002;"
                       v-if="deletingComment === false"><small>حذف دیدگاه</small></a>
                    <a href="javascript:void(0)" style="color: #ff0002;" v-else><small>حذف
                      دیدگاه</small></a>
                  </template>
                </div>
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
  import HostProfileResidenceLi from '~/components/HostProfileResidenceLi'

  export default {
    components: {
      HostProfileResidenceLi
    },
    data() {
      return {
        name: null,
        registerDate: null,
        imageUser: null,
        residences: [],
        comments: [],
        commentsCount: 0,
        totalScore: 0,
        totalAcceptPercent: 0,
        totalResponseTime: 0,
        hostScore: 0,
        totalScore1: 0,
        totalScore2: 0,
        totalScore3: 0,
        totalScore4: 0,
        totalScore5: 0,
        totalScore6: 0
      }
    },
    async asyncData({ params,$axios,redirect }) {
      let {data} = await $axios.post('api/v1/hostProfile',{'id':params.id});
      if(data.status === 0)
        redirect('/');
      return {
        name: data.name,
        registerDate: data.registerDate,
        imageUser: data.imageUser,
        residences: data.residences,
        comments: data.comments,
        commentsCount: data.commentsCount,
        totalScore: data.totalScore,
        totalAcceptPercent: data.totalAcceptPercent,
        totalResponseTime: data.totalResponseTime,
        hostScore: data.hostScore,
        totalScore1: data.totalScore1,
        totalScore2: data.totalScore2,
        totalScore3: data.totalScore3,
        totalScore4: data.totalScore4,
        totalScore5: data.totalScore5,
        totalScore6: data.totalScore6
      };
    },
    methods: {
      closeMenu() {
        const main = this.$refs.main;
        if (main.classList.contains('blur')) {
          this.$store.dispatch('setVuexMenuState',false);
          this.$store.dispatch('setVuexLogregMobileState',false);
        }
      },
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
    },
    head() {
      return {
        title: 'پروفایل میزبان - '+this.settings.sitename,
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
