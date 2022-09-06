<template>
    <div>
        <div class="title" dir="rtl" v-if="title || viewAllLink">
            <div class="name" v-if="title">
                <i class="bx bx-category"></i>
                <h3>{{ title }}</h3>
            </div>

            <div class="arrows relative" v-if="viewAllLink">
                <a :href="url+viewAllLink" class="see-more">مشاهده همه</a>
                <a @click="next" uk-slider-item="previous">
                    <i class="bx bx-chevron-right"></i>
                </a>
                <a @click="prev" uk-slider-item="next">
                    <i class="bx bx-chevron-left"></i>
                </a>
            </div>
        </div>
        <div class="flex w-full h-full">
            <swiper ref="mySwiper" :options="swiperOption" dir="rtl">
                <swiper-slide v-for="(slide, index) in slides" :key="index">
                    <slot name="slide" v-bind="{ slide }"></slot>
                </swiper-slide>
            </swiper>
        </div>
    </div>
</template>

<script>
import { Swiper, SwiperSlide } from 'vue-awesome-swiper'

import 'swiper/swiper.min.css'
import 'swiper/swiper-bundle.css'
import './style.css'
export default {
    components: {
        Swiper,
        SwiperSlide,
    },
    props: {
      slides: { type: Array, default: () => [] }, 
      title: { type: String, default: '' },
      viewAllLink: { type: String, default: '/search' },
      url: { type: String, default: 'http://localhost:3000' }
    },
    name: 'carrousel',
    data() {
        return {
            swiperOption: {
                slidesPerView: 5,
                spaceBetween: 30,
                loop: true,
                navigation: {
                    nextEl: '.swiper-button-next',
                    prevEl: '.swiper-button-prev'
                },
                breakpoints: {
                    1024: {
                        slidesPerView: 5,
                        spaceBetween: 40
                    },
                    768: {
                        slidesPerView: 4,
                        spaceBetween: 30
                    },
                    640: {
                        slidesPerView: 3,
                        spaceBetween: 20
                    },
                    320: {
                        slidesPerView: 2,
                        freeMode: true,

                        spaceBetween: 10
                    }
                },
            }
        }
    },
    computed: {
        swiper() {
            return this.$refs.mySwiper.$swiper
        }
    },
    mounted() {

    },
    methods: {
        next() {
            this.swiper.slideNext()
        },
        prev() {
            this.swiper.slidePrev()
        }
    }
}
</script>

<style>
.swiper-container {
    height: 100%;
    min-height: 0;
    min-width: 0;
    width: 100%;
    overflow: hidden;
}

.swiper-slide {
    width: auto;
    flex-shrink: 0;
    display: block;
    height: 100%;
    max-height: 100%;
}

.swiper-wrapper {
    max-height: 100%;
    height: 100%;
    display: flex;
}
</style>
