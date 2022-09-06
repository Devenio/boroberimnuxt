<template>
  <div>
    <main ref="main" class="main" :class="[vuexMenuState ? 'blur' : '',vuexLogregMobileState ? 'blur' : '']" @click="closeMenu">
      <section class="faq">
        <div class="title">
          <h1>سوالات متداول</h1>
          <hr>
          <p>پاسخ سوال های خود را در مورد {{ settings.sitename }} ، در این صفحه پیدا کنید</p>
        </div>
        <div class="items" uk-accordion>
          <div class="item uk-margin-top" v-for="faq in faqs.faqs">
            <a class="uk-accordion-title question" href="#">{{ faq.question }}</a>
            <div class="uk-accordion-content" v-html="faq.answer"></div>
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
        faqs: []
      }
    },
    async asyncData({ $axios }) {
      const { data } = await $axios.get(`api/v1/info`);
      return {
        faqs: data.data
      };
    },
    methods: {
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
        title: 'درباره ما - '+this.settings.sitename,
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
