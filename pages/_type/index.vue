<template>
  <div>
    <main ref="main" class="main" :class="[vuexMenuState ? 'blur' : '',vuexLogregMobileState ? 'blur' : '']" @click="closeMenu">
      <section class="filters-places">
        <div class="title">
          <h1>اجاره {{ type.title }}</h1>
          <hr>
          <div :class="[textshow ? `textshow` : `text`]" v-html="type.text"></div>
          <a href="javascript:void(0)" @click="textshow = true" v-if="textshow === false">بیشتر ...</a>
        </div>
        <div class="kinds">
          <swiper ref="mySwiper" :options="swiperOption" dir="rtl">
            <swiper-slide>
              <a href="javascript:void(0)" @click="showModalProvince">
                <i class="bx bxs-circle dot"></i>
                <i class="bx bxs-city"></i>
                مقصد‌اقامتگاه
              </a>
            </swiper-slide>
            <swiper-slide>
              <a href="javascript:void(0)" @click="showModalCalendar">
                <i class="bx bxs-circle dot"></i>
                <i class="bx bx-calendar"></i>
                تاریخ‌سفر
              </a>
            </swiper-slide>
            <swiper-slide>
              <a href="javascript:void(0)" @click="typesModal = true">
                <i class="bx bxs-circle dot"></i>
                <i class="bx bx-layer-plus"></i>
                نوع‌اقامتگاه
              </a>
            </swiper-slide>
            <swiper-slide>
              <a href="javascript:void(0)" @click="categoriesModal = true">
                <i class="bx bxs-circle dot"></i>
                <i class="bx bx-layer-plus"></i>
                دسته‌بندی‌ها
              </a>
            </swiper-slide>
            <swiper-slide>
              <a href="javascript:void(0)" @click="otherModal = true">
                <i class="bx bxs-circle dot"></i>
                <i class="bx bx-dots-horizontal"></i>
                فیلتر‌های‌بیشتر
              </a>
            </swiper-slide>
            <swiper-slide>
              <a href="javascript:void(0)" @click="detailsModal = true">
                <i class="bx bxs-circle dot"></i>
                <i class="bx bx-dots-horizontal"></i>
                فیلتر‌جزئیات
              </a>
            </swiper-slide>
          </swiper>
        </div>
        <div class="sorts-selected">
          <div class="sorts">
            <span>مرتب سازی بر اساس : </span>
            <div>
              <a href="javascript:void(0)" :class="form.order === 'new' ? 'active' : ''" @click="changeOrder('new')">جدیدترین‌ها</a>
              <a href="javascript:void(0)" :class="form.order === 'score' ? 'active' : ''" @click="changeOrder('score')">محبوبترین‌ها</a>
              <a href="javascript:void(0)" :class="form.order === 'cheap' ? 'active' : ''" @click="changeOrder('cheap')">ارزان‌ترین‌ها</a>
              <a href="javascript:void(0)" :class="form.order === 'expensive' ? 'active' : ''" @click="changeOrder('expensive')">گران‌ترین‌ها</a>
              <a href="javascript:void(0)" :class="form.order === 'off' ? 'active' : ''" @click="changeOrder('off')">بیشترین تخفیف</a>
            </div>
          </div>
        </div>
        <div class="total-places">
          <div class="items">
            <Residence v-for="residence in residences" :key="residence.id" :residence="residence" />
          </div>
          <div class="see-more" v-if="showMore === true">
            <a href="javascript:void(0)" @click="more" v-if="showingMore === false">مشاهده بیشتر</a>
            <a href="javascript:void(0)" v-else>در حال بارگذاری</a>
          </div>
        </div>
      </section>
    </main>
    <vk-modal center id="provinceModal" class="confirm-information-modal select-province-modal" :show.sync="provinceModal">
      <div class="title">
        <div>
          <i class="bx bx-layer-plus"></i>
          <h3>انتخاب استان</h3>
        </div>
        <vk-modal-close @click="provinceModal = false"></vk-modal-close>
      </div>
      <div class="form">
        <form @submit.prevent="">
          <div class="form-group">
            <input type="text" id="provinceName" placeholder="نام شهر یا استان" :value="provinceName" @input='e => provinceName = e.target.value' @keyup="searchProvince" />
            <button type="button" @click="searchProvince">
              <i class="bx bx-search"></i>
            </button>
          </div>
          <p class="province-title" v-if="cities != null && !!provinceName">شهر ها</p>
          <div class="lists second-list" v-if="cities != null && !!provinceName">
            <a href="javascript:void(0)" v-for="city in cities" @click="searchCity(city)">
              <i class="bx bx-notification"></i>
              {{ city.name }} ({{ city.residenceCount }} اقامتگاه)
            </a>
          </div>
          <p class="province-title">استان ها</p>
          <div class="lists" v-if="provinces != null">
            <a href="javascript:void(0)" v-for="province in provinces" @click="chooseProvince(province.id,province.name)">
              <i class="bx bx-notification"></i>
              {{ province.name }} ({{ province.cityCount }} شهر)
            </a>
          </div>
        </form>
      </div>
    </vk-modal>
    <vk-modal center id="cityModal" class="confirm-information-modal select-province-modal" :show.sync="cityModal">
      <div class="title">
        <div>
          <i class="bx bx-layer-plus"></i>
          <h3>انتخاب شهر</h3>
        </div>
        <vk-modal-close @click="cityModal = false"></vk-modal-close>
      </div>
      <div class="form">
        <form action="">
          <div class="form-group">
            <input type="text" id="cityName" placeholder="نام شهر" :value="cityName" @input='e => cityName = e.target.value' />
            <button>
              <i class="bx bx-search"></i>
            </button>
          </div>
          <div class="checkbox">
            <label for="selectedProvinceCheck">
              <input class="uk-checkbox" type="checkbox" id="selectedProvinceCheck"
                     v-model="selectedProvinceCheck" :checked="selectedProvinceCheck" @click="selectProvince" />
              جستجو در کل استان
            </label>
          </div>

          <div class="city-lists" v-if="cities != null">
            <a href="javascript:void(0)" v-bind:class="[citiesSearchNames.includes(city.name) ? 'active' : '']" v-for="city in cities" @click="addCity(city)">
              <span>
              <i class="bx bx-notification"></i>
                {{ city.name }}
              </span>
              <span>
                ({{ city.residenceCount }} اقامتگاه)
              </span>
            </a>
          </div>
          <div class="city-lists" v-else>
            <a href="javascript:void(0)">
              <i class="bx bx-notification"></i>
              چند لحظه صبر کنید
            </a>
          </div>
        </form>
      </div>
    </vk-modal>
    <vk-modal center id="calendarModal" class="confirm-information-modal select-province-modal" :show.sync="calendarModal">
      <div class="title">
        <div>
          <i class="bx bx-layer-plus"></i>
          <h3>تقویم</h3>
        </div>
        <vk-modal-close @click="calendarModal = false"></vk-modal-close>
      </div>
      <div class="form">
        <form action="" style="box-sizing: border-box !important;">
          <datepicker
            :forward-limit="settings.nextMonth"
            :backward-limit="settings.previousMonth"
            :disabled-map="{}"
            :holiday-map="{}"
            lang="Jalali"
            type="range"
            size="sm"
            :debug-selector="false"
            :enable-all-days="true"
            :days="settings.dates"
            @datemodel="handledate"
          />
        </form>
      </div>
    </vk-modal>
    <vk-modal center id="types-modal" class="modal-info-submit" :show.sync="typesModal">
      <div class="title">
        <div>
          <h3>نوع اقامتگاه</h3>
        </div>
        <vk-modal-close @click="typesModal = false"></vk-modal-close>
      </div>
      <vk-grid>
        <div v-for="type in types">
          <label :for="type.id+`type`">{{ type.title }}</label>
          <input class="uk-checkbox" :id="type.id+`type`" type="checkbox" @change="addTypes(type.id)">
        </div>
      </vk-grid>
      <div class="text">
        <div class="submit-cancel">
          <button type="button" class="submit" @click="hideTypesModal">
            <i class="bx bx-chevron-right"></i>
            جستجو
          </button>
        </div>
      </div>
    </vk-modal>
    <vk-modal center id="other-modal" class="modal-info-submit" :show.sync="otherModal">
      <div class="title">
        <p>فیلتر های بیشتر</p>
        <vk-modal-close @click="otherModal = false"></vk-modal-close>
      </div>
      <vk-grid>
        <div>
          <label for="fast">تحویل فوری</label>
          <input class="uk-checkbox" id="fast" type="checkbox" v-model="form.fast">
        </div>
        <div>
          <label for="kind">مهمان نواز</label>
          <input class="uk-checkbox" id="kind" type="checkbox" v-model="form.kind">
        </div>
      </vk-grid>
      <div class="text">
        <div class="row">
          <div class="form-group w-30">
            <input type="text" id="title" v-model="form.title" required>
            <span class="floating-label">عنوان</span>
          </div>
          <div class="form-group w-30">
            <input type="text" id="room" v-model="form.room" required>
            <span class="floating-label">تعداد اتاق</span>
          </div>
          <div class="form-group w-30">
            <input type="text" id="meter" v-model="form.meter" required>
            <span class="floating-label">متراژ</span>
          </div>
          <div class="form-group w-50">
            <input type="text" id="udistanceWalk" v-model="form.startPrice" required>
            <span class="floating-label">قیمت از</span>
          </div>
          <div class="form-group w-50">
            <input type="text" id="udistanceCar" v-model="form.endPrice" required>
            <span class="floating-label">قیمت تا</span>
          </div>
          <div class="form-group w-50">
            <input type="text" id="startPerson" v-model="form.startPerson" required>
            <span class="floating-label">تعداد نفرات از</span>
          </div>
          <div class="form-group w-50">
            <input type="text" id="endPerson" v-model="form.endPerson" required>
            <span class="floating-label">تعداد نفرات تا</span>
          </div>
        </div>
        <div class="submit-cancel">
          <button type="button" class="submit" @click="hideOtherModal">
            <i class="bx bx-chevron-right"></i>
            جستجو
          </button>
        </div>
      </div>
    </vk-modal>
    <vk-modal center id="details-modal" class="modal-info-submit" :show.sync="detailsModal">
      <div class="title">
        <p>فیلتر جزئیات</p>
        <vk-modal-close @click="detailsModal = false"></vk-modal-close>
      </div>
      <div class="w-100 uk-margin" v-for="facilityCategory in facilityCategories">
        <div class="uk-text-bold uk-margin">{{ facilityCategory.title }}</div>
        <vk-grid>
          <div v-for="facility in facilityCategory.facilities">
            <label :for="facility.id+`facility`">{{ facility.title }}</label>
            <input class="uk-checkbox" :id="facility.id+`facility`" type="checkbox" @change="addFacilities(facility.id)">
          </div>
        </vk-grid>
        <hr>
      </div>
      <div class="w-100 uk-margin" v-for="otherRule in otherRules">
        <div class="uk-text-bold uk-margin">{{ otherRule.title }}</div>
        <vk-grid>
          <div v-for="otherRuleDetail in otherRule.details">
            <label :for="otherRuleDetail.id+`otherRuleDetail`">{{ otherRuleDetail.text }}</label>
            <input class="uk-checkbox" :id="otherRuleDetail.id+`otherRuleDetail`" type="checkbox" @change="addOtherRules(otherRuleDetail.id)">
          </div>
        </vk-grid>
        <hr>
      </div>
      <div class="text">
        <div class="submit-cancel">
          <button type="button" class="submit" @click="hideDetailsModal">
            <i class="bx bx-chevron-right"></i>
            جستجو
          </button>
        </div>
      </div>
    </vk-modal>
    <vk-modal center id="categories-modal" class="modal-info-submit" :show.sync="categoriesModal">
      <div class="title">
        <div>
          <h3>دسته‌بندی‌ها</h3>
        </div>
        <vk-modal-close @click="categoriesModal = false"></vk-modal-close>
      </div>
      <vk-grid>
        <div v-for="category in settings.categories">
          <label :for="category.id+`category`">{{ category.title }}</label>
          <input class="uk-checkbox" :id="category.id+`category`" type="checkbox" @change="addCategories(category.id)">
        </div>
      </vk-grid>
      <div class="text">
        <div class="submit-cancel">
          <button type="button" class="submit" @click="hideCategoriesModal">
            <i class="bx bx-chevron-right"></i>
            جستجو
          </button>
        </div>
      </div>
    </vk-modal>
  </div>
</template>

<style src="~/assets/styles/css/residences.css" scoped />
<style scoped>
  .swiper-slide {
    margin-left: 0!important;
  }
  @media screen and (min-width: 768px) {
    .swiper-slide {
      width: 21% !important;
    }
  }
  @media screen and (max-width: 768px) {
    .swiper-slide {
      width: 33% !important;
    }
  }
  @media screen and (max-width: 500px) {
    .swiper-slide {
      width: 80% !important;
    }
  }
</style>

<script>
  import { Swiper, SwiperSlide } from 'vue-awesome-swiper'
  import 'swiper/swiper.min.css'
  import 'swiper/swiper-bundle.css'
  import Residence from "~/components/Residence";
  import datepicker from '@/components/datepicker3';
  import { mapGetters } from 'vuex';

  export default {
    components: {
      Swiper,
      SwiperSlide,
      Residence,
      datepicker
    },
    data() {
      return {
        swiperOption: {
          slidesPerView: 4,
          spaceBetween: 30,
          loop: true,
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev'
          },
          breakpoints: {
            768: {
              slidesPerView: 4,
              spaceBetween: 30
            },
            640: {
              slidesPerView: 3,
              spaceBetween: 20
            },
            500: {
              slidesPerView: 1,
              freeMode: true,
              spaceBetween: 10
            }
          },
        },
        provinceModal: false,
        cityModal: false,
        calendarModal: false,
        typesModal: false,
        otherModal: false,
        detailsModal: false,
        categoriesModal: false,
        textshow: false,
        date: null,
        residences: [],
        type: [],
        provinces: [],
        types: [],
        cities: [],
        facilityCategories: [],
        otherRules: [],
        citiesSearchNames: [],
        total: 0,
        showMore: true,
        selectedProvinceCheck: false,
        selectedProvinceID: false,
        selectedProvinceName: null,
        provinceName: null,
        cityName: null,
        showingMore: false,
        form: {
          fast: false,
          kind: false,
          title: null,
          city: null,
          order: 'new',
          types: [],
          categories: [],
          cities: [],
          otherRules: [],
          facilities: [],
          dates: [],
          startDate: null,
          endDate: null,
          startPrice: null,
          endPrice: null,
          startPerson: null,
          province: null,
          provinceName: null,
          endPerson: null,
          room: null,
          meter: null,
          start: 0
        }
      }
    },
    async asyncData({ route,params,$axios,redirect }) {
      let {data} = await $axios.post('api/v1/typeResidences',{'slug':params.type});
      if(data.status === 0)
        redirect('/404');
      return {
        residences: data.data.residences,
        type: data.type,
        provinces: data.data.provinces,
        types: data.data.types,
        cities: data.data.cities,
        facilityCategories: data.data.facilityCategories,
        otherRules: data.data.otherRules,
        total: data.data.total,
        form: {
          id:params.id,
          slug:params.type,
          title:route.query.title,
          startPerson:route.query.startPerson,
          province:route.query.province,
          provinceName:route.query.provinceName,
          city: route.query.city,
          order: 'new',
          types: [],
          categories: [],
          cities: [],
          otherRules: [],
          facilities: [],
          dates: [],
          startDate: null,
          endDate: null,
          startPrice: null,
          endPrice: null,
          endPerson: null,
          room: null,
          meter: null,
          start: 0
        }
      }
    },
    methods: {
      addTypes (id) {
        if(!this.form.types.includes(id))
          this.form.types.push(id);
        else
          this.form.types.splice(this.form.types.indexOf(id), 1);
      },
      addFacilities(id) {
        if(!this.form.facilities.includes(id))
          this.form.facilities.push(id);
        else
          this.form.facilities.splice(this.form.facilities.indexOf(id), 1);
      },
      addOtherRules(id) {
        if(!this.form.otherRules.includes(id))
          this.form.otherRules.push(id);
        else
          this.form.otherRules.splice(this.form.otherRules.indexOf(id), 1);
      },
      addCategories (id) {
        if(!this.form.categories.includes(id))
          this.form.categories.push(id);
        else
          this.form.categories.splice(this.form.categories.indexOf(id), 1);
      },
      async changeOrder(order) {
        this.residences = [];
        this.showMore = true;
        this.form.start += 0;
        this.form.order = order;
        const { data } = await this.$axios.post(`api/v1/typeResidences`,this.form);
        this.residences = data.data.residences;
        this.total = data.data.total;
      },
      async search() {
        this.form.start = 0;
        this.residences = [];
        this.showMore = true;
        const { data } = await this.$axios.post(`api/v1/typeResidences`,this.form);
        this.residences = data.data.residences;
        this.total = data.data.total;
      },
      async more() {
        this.showingMore = true;
        this.form.start += 8;
        const { data } = await this.$axios.post(`api/v1/typeResidences`,this.form);
        if(data.data.residences.length > 0) {
          this.residences.concat(data.data.residences);
          this.total = data.data.total;
        } else
          this.showMore = false;
        this.showingMore = false;
      },
      hideOtherModal() {
        this.search();
        this.otherModal = false;
      },
      hideTypesModal() {
        this.search();
        this.typesModal = false;
      },
      hideCategoriesModal() {
        this.search();
        this.categoriesModal = false;
      },
      hideDetailsModal() {
        this.search();
        this.detailsModal = false;
      },
      showModalCalendar() {
        this.calendarModal = true;
      },
      async showModalProvince() {
        this.provinces = null;
        this.cities = null;
        this.provinceModal = true;
        this.form.cities = [];
        this.citiesSearchNames = [];
        this.selectedProvinceID = null;
        this.selectedProvinceName = null;
        this.selectedProvinceCheck = false;
        this.cityName = null;
        let provinces = await this.$axios.post('api/v1/searchProvince',{name:this.selectedProvinceName,mark:1});
        this.provinces = provinces.data.provinces;
      },
      async chooseProvince(id,name) {
        this.cities = null;
        this.provinceModal = false;
        this.cityModal = true;
        this.citiesSearchNames = [];
        this.selectedProvinceID = id;
        this.selectedProvinceName = name;
        let cities = await this.$axios.post('api/v1/searchCity',{id:this.selectedProvinceID});
        if(cities.data.status === 1) {
          this.cities = cities.data.cities;
        }
      },
      addCity (city) {
        if(!this.citiesSearchNames.includes(city.name))
          this.citiesSearchNames.push(city.name);
        else
          this.citiesSearchNames.splice(this.citiesSearchNames.indexOf(city.name), 1);
        if(!this.form.cities.includes(city.id))
          this.form.cities.push(city.id);
        else
          this.form.cities.splice(this.form.cities.indexOf(city.id), 1);
        this.search();
      },
      async searchProvince() {
        this.cities = [];
        let provinces = await this.$axios.post('api/v1/searchProvince',{name:this.provinceName});
        if(provinces.data.status === 1) {
          this.provinces = provinces.data.provinces;
          this.cities = provinces.data.cities;
        } else if(provinces.data.status === 422) {
          this.errors = provinces.data.errors;
          this.$snotify.warning(provinces.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(provinces.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
      },
      searchCity(city) {
        this.provinceModal = false;
        this.$router.push('/cities/'+city.id+`/`+city.name.replace(' ','-'));
      },
      selectProvince() {
        this.selectedProvinceCheck = !this.selectedProvinceCheck;
        if(this.selectedProvinceCheck === true) {
          this.form.province = this.selectedProvinceID;
          this.form.provinceName = this.selectedProvinceName;
          this.cityModal = false;
          this.search();
        }
      },
      number_3_3(num, sep) {
        const number = typeof num === 'number' ? num.toString() : num
        const separator = typeof sep === 'undefined' ? ',' : sep
        return number.replace(/(\d)(?=(\d\d\d)+(?!\d))/g, '$1' + separator)
      },
      closeMenu() {
        const main = this.$refs.main;
        if (main.classList.contains('blur')) {
          this.$store.dispatch('setVuexMenuState',false);
          this.$store.dispatch('setVuexLogregMobileState',false);
        }
      },
      handledate(e) {
        this.date = e
      }
    },
    computed: {
      dateObj() {
        return this.date?.dates
      },
      vuexMenuState() {
        return this.$store.getters.vuexMenuState
      },
      vuexLogregMobileState() {
        return this.$store.getters.vuexLogregMobileState
      },
      ...mapGetters(['settings'])
    },
    watch: {
      date: {
        deep: true,
        handler(e) {
          if (e.dates.length === 2) {
            this.form.dates = this.date.dates;
            this.calendarModal = false;
            this.search();
          }
        },
      },
    },
    created() {
      this.$store.dispatch('setVuexMenuState',false);
    },
    head() {
      return {
        title: 'اجاره '+this.type.title,
        meta: [
          {
            hid: 'description',
            name: 'description',
            content: this.type.metaDesc
          },
        ],
      }
    }
  }
</script>
