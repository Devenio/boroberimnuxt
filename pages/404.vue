<template>
  <div>
    <main ref="main" class="main" :class="[vuexMenuState ? 'blur' : '',vuexLogregMobileState ? 'blur' : '']" @click="closeMenu">
      <section class="filters-places">
        <div class="title">
          <h1>خطای 404</h1>
          <hr>
          صفحه مورد نظر شما یافت نشد یا ممکن است حذف شده باشد.
        </div>
      </section>
    </main>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex';

  export default {
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
    }
  }
</script>
