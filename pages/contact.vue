<template>
  <div>
    <main ref="main" class="main" :class="[vuexMenuState ? 'blur' : '',vuexLogregMobileState ? 'blur' : '']" @click="closeMenu">
      <section class="contact-us">
        <div class="title">
          <h1>تماس با ما</h1>
          <hr>
          <div v-html="contact.contact"></div>
        </div>
        <div class="body">
          <div class="text">
            <h3>راه های ارتباطی</h3>
            <hr>
            <div class="items">
              <div>
                <i class="bx bx-headphone"></i>
                <div>
                  <span>تلفن پشتیبانی : </span>
                  <p>{{ settings.phone }}</p>
                </div>
              </div>
              <div>
                <i class="bx bx-envelope"></i>
                <div>
                  <span>ایمیل : </span>
                  <p>{{ settings.email }}</p>
                </div>
              </div>
              <div>
                <i class="bx bx-current-location"></i>
                <div>
                  <span>آدرس دفتر : </span>
                  <p>{{ settings.address }}</p>
                </div>
              </div>
            </div>
          </div>
          <div class="form">
            <h3>فرم تماس با ما</h3>
            <hr>
            <form action="">
              <div class="form-group">
                <label for="name">نام و نام خانوادگی</label>
                <input type="text" id="name" v-model="form.name">
                <span v-if="errors.name" class="error">{{ errors.name[0] }}</span>
              </div>
              <div class="form-group">
                <label for="subject">موضوع</label>
                <input type="text" id="subject" v-model="form.subject">
                <span v-if="errors.subject" class="error">{{ errors.subject[0] }}</span>
              </div>
              <div class="form-group">
                <label for="email">ایمیل</label>
                <input type="text" id="email" v-model="form.email">
                <span v-if="errors.email" class="error">{{ errors.email[0] }}</span>
              </div>
              <div class="form-group">
                <label for="mobile">تلفن تماس</label>
                <input type="text" id="mobile" v-model="form.mobile">
                <span v-if="errors.mobile" class="error">{{ errors.mobile[0] }}</span>
              </div>
              <div class="form-group description">
                <label for="text">متن پیام</label>
                <textarea id="text" rows="10" v-model="form.text"></textarea>
                <span v-if="errors.text" class="error">{{ errors.text[0] }}</span>
              </div>
              <div class="submit">
                <button type="button" v-if="sendingContact === false" @click="sendContact">ارسال درخواست</button>
                <button type="button" v-else>در حال ارسال</button>
              </div>
            </form>
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
    data() {
      return {
        contact: [],
        errors: [],
        sendingContact: false,
        form: {
          name: null,
          subject: null,
          email: null,
          mobile: null,
          text: null
        }
      }
    },
    async asyncData({ $axios }) {
      const { data } = await $axios.get(`api/v1/info`);
      return {
        contact: data.data
      };
    },
    methods: {
      async sendContact() {
        this.sendingContact = true;
        this.errors = [];
        let contact = await this.$axios.post('api/v1/sendContact',this.form);
        if(contact.data.status === 1) {
          this.$snotify.success(contact.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.form = {
            name : null,
            subject : null,
            email : null,
            mobile : null,
            text : null
          };
        } else if(contact.data.status === 422) {
          this.errors = contact.data.errors;
          this.$snotify.warning(contact.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(contact.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.sendingContact = false;
      },
      closeMenu() {
        const main = this.$refs.main;
        if (main.classList.contains('blur')) {
          this.$store.dispatch('setVuexMenuState',false);
          this.$store.dispatch('setVuexLogregMobileState',false);
        }
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
    },
    head() {
      return {
        title: 'ارتباط با ما - '+this.settings.sitename,
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
