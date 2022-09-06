<template>
  <div>
    <main ref="main" class="main" :class="[vuexMenuState ? 'blur' : '',vuexLogregMobileState ? 'blur' : '']" @click="closeMenu">
      <section class="about-us">
        <div class="title">
          <h1>قوانین و مقررات</h1>
          <hr>
          <p>قوانین و مقررات وب سایت {{ settings.sitename }}</p>
        </div>
        <div class="text" v-html="rules.rules"></div>
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
        rules: []
      }
    },
    async asyncData({ $axios }) {
      const { data } = await $axios.get(`api/v1/info`);
      return {
        rules: data.data
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
        title: 'قوانین و مقررات - '+this.settings.sitename,
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
