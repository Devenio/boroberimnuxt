<template>
  <div>
    <main class="main" :class="[vuexMenuState ? 'blur' : '',vuexLogregMobileState ? 'blur' : '']" ref="main" @click="closeMenu">
      <section class="detail-place">
        <div>
          <div class="reserve-request-responsive">
            <a @click="showReserveRequestResponsiveModal" href="javascript:void(0)">درخواست
              رزرو</a>
          </div>

          <div class="body" id="body">
            <div class="title-rate-share">
              <div class="title">
                <h1>{{ residence.title }}</h1>
              </div>
              <div class="rate-share">
                <div class="rate">
                  <span><i class="bx bxs-star"></i></span>
                  <p>{{ residence.score }}</p>
                </div>
                <div class="comment">
                  <i class="bx bx-comment-detail"></i>
                  <p>{{ residence.commentsCount }} دیدگاه</p>
                </div>
                <a @click="shareModal = true" href="javascript:void(0)">
                  <i class="bx bx-share-alt"></i>
                </a>
                <a @click="addToFavorite(residence.id)" href="javascript:void(0)"
                   v-if="$auth.loggedIn">
                  <i class="bx bxs-heart" v-if="residence.favorite === 1"></i>
                  <i class="bx bx-heart" v-else></i>
                </a>
              </div>
            </div>

            <div class="photos">
              <div class="uk-position-relative uk-visible-toggle" tabindex="-1"
                   uk-slideshow="min-height: 50; max-height: 350;autoplay: true;pause-on-hover: true;autoplay-interval: 2500">
                <ul class="uk-slideshow-items" uk-lightbox="animation: slide">
                  <li v-for="image in residence.albumImages">
                    <a :href="image">
                      <img :alt="residence.title" :src="image">
                    </a>
                  </li>
                </ul>
                <a class="uk-position-center-left uk-position-small" href="javascript:void(0)"
                   uk-slidenav-next
                   uk-slideshow-item="next"></a>
                <a class="uk-position-center-right uk-position-small" href="javascript:void(0)"
                   uk-slidenav-previous
                   uk-slideshow-item="previous"></a>
              </div>
            </div>

            <div class="host-info">
              <div class="name-img">
                <div class="img">
                  <img :alt="residence.title" :src="residence.imageUser">
                </div>
                <div class="name">
                  <p>{{ residence.user }}</p>
                  <span>
                      <i class="bx bxs-badge-check"></i>
                      میزبان شما
                  </span>
                </div>
              </div>
              <nuxt-link :to="`/hostProfile/`+residence.userId">
                <i class="bx bx-plus"></i>
                مشاهده صفحه میزبان
              </nuxt-link>
            </div>

            <div class="about">
              <div class="address-title">
                <div class="about-title">
                  <i class="bx bx-dots-horizontal-rounded"></i>
                  <h3>درباره اقامتگاه</h3>
                </div>
                <div class="address-code">
                  <div class="address">
                    <i class="bx bx-current-location"></i>
                    <p>
                      <nuxt-link :to="`/`+residence.typeSlug+`/`+residence.provinceSlug">
                        {{ residence.province }}
                      </nuxt-link>
                      ،
                      <nuxt-link :to="`/`+residence.typeSlug+`/`+residence.provinceSlug+`/`+residence.citySlug">{{
                        residence.city }}
                      </nuxt-link>
                    </p>
                  </div>
                  <div class="code">
                    <span>کد آگهی :</span>
                    <p>{{ residence.id }}</p>
                  </div>
                </div>
              </div>

              <div class="aboutText" v-html="residence.detail.text"></div>

              <div class="text-options">
                <div class="option">
                  <span class="first"> مساحت : </span>
                  <span class="second">{{ residence.detail.meter }} متر</span>
                </div>
                <div class="option">
                  <span class="first">کل طبقه : </span>
                  <span class="second">{{ residence.detail.allFloor }}</span>
                </div>
              </div>

              <div class="room-items">
                <div>
                  <i class="bx bx-home-smile"></i>
                  <p v-if="residence.detail.floor === 0">طبقه همکف</p>
                  <p v-else>طبقه {{ residence.detail.floor }}</p>
                </div>
                <div>
                  <i class="bx bx-home-smile"></i>
                  <p>زیربنا : {{ residence.detail.buildingMeter }}</p>
                </div>
                <div>
                  <i class="bx bx-home-smile"></i>
                  <p>{{ residence.detail.room }} اتاق</p>
                </div>
              </div>
            </div>

            <div class="options" v-for="facilityCategory in residence.facilityCategories.slice(0, 1)"
                 v-if="facilityCategory.facilities.length > 0">
              <h3>{{ facilityCategory.title }}</h3>
              <div class="items">
                <div class="item" v-for="facility in facilityCategory.facilities.slice(0, 4)">
                  <i :class="facility.icon+` md-size`"></i>
                  <span>{{ facility.title }}</span>
                </div>
              </div>
              <a href="javascript:void(0)" class="see-more" @click="facilityModal = true">
                مشاهده همه امکانات
                <i class="bx bx-chevron-down"></i>
              </a>
              <vk-modal :show.sync="facilityModal" center class="bath-modal" id="room-modal">
                <div class="together-row">
                  <div class="each-bath w-100">
                    <div class="second inline-second" v-for="facilityCategory in residence.facilityCategories"
                         v-if="facilityCategory.facilities.length > 0">
                      <p>{{ facilityCategory.title }}</p>
                      <ul>
                        <li v-for="facility in facilityCategory.facilities">
                          <i :class="facility.icon+` md-size`"></i>
                          {{ facility.title }}
                        </li>
                      </ul>
                    </div>
                  </div>
                </div>
              </vk-modal>
            </div>

            <div class="options">
              <h3>مسیرهای دسترسی</h3>
              <div class="items">
                <div class="item" v-for="roadOption in residence.roadOptions.slice(0, 4)">
                  <i :class="roadOption.icon+` md-size`"></i>
                  <span>{{ roadOption.title }}</span>
                </div>
              </div>
              <a href="javascript:void(0)" class="see-more" @click="roadOptionModal = true">
                مشاهده همه مسیرهای دسترسی
                <i class="bx bx-chevron-down"></i>
              </a>
              <vk-modal :show.sync="roadOptionModal" center class="bath-modal" id="road-modal">
                <div class="together-row">
                  <div class="each-bath w-100">
                    <div class="second inline-second">
                      <ul>
                        <li v-for="roadOption in residence.roadOptions">
                          <i :class="roadOption.icon+` md-size`"></i>
                          {{ roadOption.title }}
                        </li>
                      </ul>
                    </div>
                  </div>
                </div>
              </vk-modal>
            </div>

            <div class="options">
              <h3>شرایط محیطی</h3>
              <div class="items">
                <div class="item" v-for="enviromentalOption in residence.enviromentalOptions.slice(0, 4)">
                  <i :class="enviromentalOption.icon+` md-size`"></i>
                  <span>{{ enviromentalOption.title }}</span>
                </div>
              </div>
              <a href="javascript:void(0)" class="see-more" @click="enviromentalOptionModal = true">
                مشاهده همه شرایط محیطی
                <i class="bx bx-chevron-down"></i>
              </a>
              <vk-modal :show.sync="enviromentalOptionModal" center class="bath-modal" id="enviromental-modal">
                <div class="together-row">
                  <div class="each-bath w-100">
                    <div class="second inline-second">
                      <ul>
                        <li v-for="enviromentalOption in residence.enviromentalOptions">
                          <i :class="enviromentalOption.icon+` md-size`"></i>
                          {{ enviromentalOption.title }}
                        </li>
                      </ul>
                    </div>
                  </div>
                </div>
              </vk-modal>
            </div>

            <div class="about about-bath">
              <div class="about-title">
                <i class="bx bx-dots-horizontal-rounded"></i>
                <h3>نزدیک ترین اماکن</h3>
              </div>

              <p>
                برای مشاهده اطلاعات هر مکان بر روی آن کلیک کنید .
              </p>

              <div class="room-items">
                <a @click="setNearbyPlace(nearbyPlace)" href="javascript:void(0)"
                   v-for="nearbyPlace in residence.nearbyPlaces">
                  <i class="bx bx-bus"></i>
                  <p>{{ nearbyPlace.title }}</p>
                </a>
              </div>

              <vk-modal :show.sync="nearbyPlaceModal" center class="bath-modal"
                        id="nearbyPlace-modal">
                <div class="each-bath">
                  <h3>{{ nearbyPlace.title }}</h3>
                  <div class="first" v-if="nearbyPlace.distanceCar !== null">
                    <p>فاصله با خودرو : </p>
                    <span>{{ nearbyPlace.distanceCar }} دقیقه</span>
                  </div>
                  <div class="first" v-if="nearbyPlace.distanceWalk !== null">
                    <p>فاصله پیاده : </p>
                    <span>{{ nearbyPlace.distanceWalk }} دقیقه</span>
                  </div>
                </div>
              </vk-modal>
            </div>

            <div class="about about-bath">
              <div class="about-title">
                <i class="bx bx-dots-horizontal-rounded"></i>
                <h3>پارکینگ ها</h3>
              </div>

              <p v-show="residence.detail.parkingText !== null" v-html="residence.detail.parkingText"></p>
              <p v-show="residence.detail.parkingPublic === 1">پارکینگ عمومی در نزدیکی اقامتگاه وجود دارد</p>
              <p v-show="residence.detail.parkingOutside === 1">جای پارکینگ در بیرون از اقامتگاه است</p>

              <div class="room-items">
                <a @click="setParking(parking)" href="javascript:void(0)"
                   v-for="parking in residence.parkings">
                  <i class="bx bx-car"></i>
                  <p v-if="parking.type === 1">سرپوشیده</p>
                  <p v-else>روباز</p>
                </a>
              </div>

              <vk-modal :show.sync="parkingModal" center class="bath-modal"
                        id="nearbyPlace-modal">
                <div class="each-bath">
                  <h3 v-if="parking.type === 1">سرپوشیده</h3>
                  <h3 v-else>روباز</h3>
                  <div class="first">
                    <p>تعداد : </p>
                    <span>{{ parking.count }}</span>
                  </div>
                  <div class="first">
                    <p>پارکینگ مزاحم است : </p>
                    <span v-if="parking.busy === 1">بله</span>
                    <span v-else>خیر</span>
                  </div>
                  <div class="first">
                    <p>پارکینگ مناسب خودرو های سدان : </p>
                    <span v-if="parking.tiny === 1">بله</span>
                    <span v-else>خیر</span>
                  </div>
                </div>
              </vk-modal>
            </div>

            <div class="about about-bath">
              <div class="about-title">
                <i class="bx bx-dots-horizontal-rounded"></i>
                <h3>اتاق‌ها</h3>
              </div>

              <p>
                برای مشاهده ویژگی‌ها و امکانات هر اتاق بر روی آن کلیک کنید .
              </p>

              <div class="room-items">
                <a @click="setRoom(room)" href="javascript:void(0)" v-for="room in residence.rooms">
                  <i class="bx bx-home-smile"></i>
                  <p>{{ room.title }}</p>
                </a>
              </div>

              <vk-modal :show.sync="roomModal" center class="bath-modal" id="room-modal">
                <div class="together-row">
                  <div class="each-bath w-50">
                    <h3>{{ room.title }}</h3>
                    <div class="first success" v-if="room.masterRoom === 1">
                      <p>اتاق مستر : </p>
                      <i class="bx bxs-check-circle"></i> می‌باشد
                    </div>
                    <div class="first error" v-else>
                      <p>اتاق مستر : </p>
                      <i class="bx bx-error"></i> نمی‌باشد
                    </div>
                    <div class="first">
                      <p>طبقه اتاق: </p>
                      <span v-if="room.floor === 0">همکف</span>
                      <span v-else>{{ room.floor }}</span>
                    </div>
                  </div>
                  <hr>
                  <div class="each-bath w-50">
                    <div class="first count" v-show="room.onebed > 0">
                      <p>تخت‌خواب یکنفره : </p>
                      <span>{{ room.onebedCount }} عدد</span>
                    </div>
                    <div class="first count" v-show="room.twobed > 0">
                      <p>تخت‌خواب دونفره : </p>
                      <span>{{ room.twobedCount }} عدد</span>
                    </div>
                    <div class="first count" v-show="room.sofabed >= 1">
                      <p>مبل تخت‌خواب‌شو : </p>
                      <span>{{ room.sofabedCount }} عدد</span>
                    </div>
                  </div>
                  <hr>
                  <div class="each-bath w-100">
                    <div class="second inline-second">
                      <p>سیستم‌ گرمایشی اتاق : </p>
                      <ul>
                        <li v-for="heater in room.heaters">
                          <i :class="`big-icon `+heater.icon"></i>
                          {{ heater.title }}
                        </li>
                      </ul>
                    </div>
                    <div class="second inline-second">
                      <p>سیستم سرمایشی اتاق : </p>
                      <ul>
                        <li v-for="cooler in room.coolers">
                          <i :class="`big-icon `+cooler.icon"></i>
                          {{ cooler.title }}
                        </li>
                      </ul>
                    </div>
                  </div>
                </div>
              </vk-modal>
            </div>

            <div class="about about-bath">
              <div class="about-title">
                <i class="bx bx-dots-horizontal-rounded"></i>
                <h3>سرویس‌های بهداشتی</h3>
              </div>

              <p>
                برای مشاهده امکانات سرویس‌های بهداشتی بر روی آن کلیک کنید .
              </p>

              <div class="room-items">
                <a @click="setWc(wc)" href="javascript:void(0)" v-for="wc in residence.wcs">
                  <i class="bx bx-reset"></i>
                  <p>{{ wc.title }}</p>
                </a>
              </div>
              <div class="other-bath-options">
                <div class="item" v-for="hygieneOption in residence.hygieneOptions">
                  <i :class="hygieneOption.icon+` md-size`"></i>
                  <span>{{ hygieneOption.title }}</span>
                </div>
              </div>

              <vk-modal :show.sync="wcModal" center class="bath-modal" id="wc-modal">
                <div class="each-bath">
                  <h3>{{ wc.title }}</h3>
                  <div class="first">
                    <p>موقعیت : </p>
                    <span>{{ wc.position }}</span>
                  </div>
                  <div class="first success" v-if="wc.share === 1">
                    <p>اشتراکی : </p>
                    <i class="bx bxs-check-circle"></i> بله
                  </div>
                  <div class="first error" v-else>
                    <p>اشتراکی : </p>
                    <i class="bx bx-error"></i> خیر
                  </div>
                </div>
                <div class="each-bath">
                  <div class="second inline-second">
                    <p>امکانات بهداشتی : </p>
                    <ul>
                      <li v-for="option in wc.options">
                        <i :class="`big-icon `+option.icon"></i>
                        {{ option.title }}
                      </li>
                    </ul>
                  </div>
                </div>
              </vk-modal>
            </div>

            <div class="about about-bath">
              <div class="about-title">
                <i class="bx bx-dots-horizontal-rounded"></i>
                <h3>وضعیت آنتن دهی</h3>
              </div>
              <p>
                وضعیت آنتن‌دهی هر یک از اپراتورها در این اقامتگاه:
              </p>
              <div class="room-items">
                <a href="javascript:void(0)" v-show="residence.detail.hamrahavalCoverage !== null">
                  <strong>همراه اول</strong>
                  <p>{{ residence.detail.hamrahavalCoverage }}</p>
                </a>
                <a href="javascript:void(0)" v-show="residence.detail.irancellCoverage !== null">
                  <strong>ایرانسل</strong>
                  <p>{{ residence.detail.irancellCoverage }}</p>
                </a>
                <a href="javascript:void(0)" v-show="residence.detail.raitellCoverage !== null">
                  <strong>رایتل</strong>
                  <p>{{ residence.detail.raitellCoverage }}</p>
                </a>
              </div>
            </div>

            <div class="rules">
              <h3>قوانین و مقررات</h3>
              <div class="flex flex-rules">
                <div class="box" v-show="residence.detail.startDeliveryTime !== null">
                  <strong>ساعت ورود</strong>
                  <img alt="" src="/images/icon-back.png"/>
                  <span>از {{ residence.detail.startDeliveryTime }} تا {{ residence.detail.endDeliveryTime }}</span>
                </div>
                <div class="box" v-show="residence.detail.evacuateTime !== null">
                  <strong>ساعت خروج</strong>
                  <img alt="" src="/images/icon-back.png"/>
                  <span>{{ residence.detail.evacuateTime }}</span>
                </div>
                <div class="box" v-show="residence.detail.minReserve !== null">
                  <strong>حداقل مدت اقامت</strong>
                  <img alt="" src="/images/icon-time.png"/>
                  <span>{{ residence.detail.minReserve }} شب</span>
                </div>
                <div class="box" v-show="residence.detail.maxReserve !== null">
                  <strong>حداکثر مدت اقامت</strong>
                  <img alt="" src="/images/icon-time.png"/>
                  <span>{{ residence.detail.maxReserve }} شب</span>
                </div>
              </div>
              <div class="points">
                <div class="points" v-for="otherRule in residence.otherRules">
                  <div class="pointsBlack points-positive">
                    <p class="uk-margin-bottom">
                      <img alt="" src="/images/icon-check.png"/>
                      {{ otherRule.title }} : {{ otherRule.text }}
                    </p>
                  </div>
                  <br>
                </div>
                <div v-for="rule in residence.rules">
                  <div @click="showRule(`قوانین گهنسالان و نوزاد ها`,rule.text)"
                       class="points points-positive">
                    <p class="uk-margin-bottom" v-show="rule.type === 1">
                      <img alt="" src="/images/icon-check.png"/>
                      قوانین گهنسالان و نوزاد ها
                    </p>
                  </div>
                  <div @click="showRule(`قوانین افراد معلول`,rule.text)"
                       class="points points-positive">
                    <p class="uk-margin-bottom" v-show="rule.type === 2">
                      <img alt="" src="/images/icon-check.png"/>
                      قوانین افراد معلول
                    </p>
                  </div>
                  <div @click="showRule(`سایر قوانین`,rule.text)" class="points points-positive">
                    <p class="uk-margin-bottom" v-show="rule.type === 3">
                      <img alt="" src="/images/icon-check.png"/>
                      سایر قوانین
                    </p>
                  </div>
                  <br>
                </div>
              </div>
            </div>

            <div class="rules" v-if="residence.lat !== 0">
              <h3>محل اقامتگاه روی نقشه</h3>
              <p>
                محل دقیق اقامتگاه پس از تکمیل و نهایی شدن رزرو برای شما ارسال خواهد شد.
              </p>
              <div id="map-wrap" style="height: 300px; margin-bottom: 20px; width: 100%;">
                <client-only>
                  <l-map :center="[residence.lat,residence.lng]" :zoom=13>
                    <l-tile-layer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></l-tile-layer>
                    <l-marker :lat-lng="[residence.lat,residence.lng]"
                              v-if="residence.buy !== 0"></l-marker>
                    <l-circle :lat-lng="[residence.lat,residence.lng]"
                              :radius="300"
                              v-else
                    />
                  </l-map>
                </client-only>
              </div>
              <div style="margin-bottom: 20px; width: 100%;" v-if="residence.buy !== 0">
                <a :href="`https://www.google.com/maps/?q=`+residence.lat+`,`+residence.lng"
                   class="uk-button uk-button-default" rel="nofollow" style="width: 90%;">باز کردن لوکیشن</a>
              </div>
            </div>

            <div class="comments" id="comments">
              <div class="title">
                <i class="bx bx-comment-detail"></i>
                <h3>دیدگاه کاربران</h3>
              </div>
              <div class="feedback">
                <div class="until-now">
                  <span>
                      <i class="bx bxs-star"></i>
                  </span>
                  <p>تاکنون {{ residence.commentsCount }} دیدگاه ثبت شده است</p>
                </div>
                <div class="rate-parts">
                  <div class="parts">
                    <ul>
                      <li>
                        <i class="bx bxs-circle"></i>
                        <p class="name">دسترسی</p>
                        <p class="rate">{{ residence.score1 }}</p>
                      </li>
                      <li>
                        <i class="bx bxs-circle"></i>
                        <p class="name">تطابق با آگهی در سایت</p>
                        <p class="rate">{{ residence.score2 }}</p>
                      </li>
                      <li>
                        <i class="bx bxs-circle"></i>
                        <p class="name">نظافت</p>
                        <p class="rate">{{ residence.score3 }}</p>
                      </li>
                      <li>
                        <i class="bx bxs-circle"></i>
                        <p class="name">همسایگی</p>
                        <p class="rate">{{ residence.score4 }}</p>
                      </li>
                      <li>
                        <i class="bx bxs-circle"></i>
                        <p class="name">نسبت ارزش به قیمت</p>
                        <p class="rate">{{ residence.score5 }}</p>
                      </li>
                      <li>
                        <i class="bx bxs-circle"></i>
                        <p class="name">کیفیت میزبانی</p>
                        <p class="rate">{{ residence.score6 }}</p>
                      </li>
                    </ul>
                  </div>
                  <div class="total-rate">
                    <p>{{ residence.score }}</p>
                    <div class="see-more">
                      <a @click="commentModal = true" href="javascript:void(0)"
                         v-if="residence.buy !== 0">
                        <i class="bx bx-plus"></i>
                        ثبت دیدگاه جدید
                      </a>
                      <a @click="noCommentModal = true" href="javascript:void(0)" v-else>ثبت دیدگاه
                        جدید</a>
                    </div>
                  </div>
                </div>
                <vk-modal :show.sync="commentModal" center class="send-comment-modal uk-modal-container"
                          id="commentModal">
                  <h3 class="uk-modal-title">ثبت دیدگاه جدید</h3>
                  <div class="text">
                    <form action="#">
                      <div class="stars">
                        <div class="each">
                          <span>دسترسی</span>
                          <div>
                            <no-ssr>
                              <star-rating :increment="1" :rating="3" :rtl="true"
                                           :show-rating="false" :star-size="20"
                                           @rating-selected="setRating1" active-color="#1749FF"/>
                            </no-ssr>
                          </div>
                        </div>
                        <div class="each">
                          <span>تطابق با آگهی در سایت</span>
                          <div>
                            <no-ssr>
                              <star-rating :increment="1" :rating="3" :rtl="true"
                                           :show-rating="false" :star-size="20"
                                           @rating-selected="setRating2" active-color="#1749FF"/>
                            </no-ssr>
                          </div>
                        </div>
                        <div class="each">
                          <span>نظافت</span>
                          <div>
                            <no-ssr>
                              <star-rating :increment="1" :rating="3" :rtl="true"
                                           :show-rating="false" :star-size="20"
                                           @rating-selected="setRating3" active-color="#1749FF"/>
                            </no-ssr>
                          </div>
                        </div>
                        <div class="each">
                          <span>همسایگی</span>
                          <div>
                            <no-ssr>
                              <star-rating :increment="1" :rating="3" :rtl="true"
                                           :show-rating="false" :star-size="20"
                                           @rating-selected="setRating4" active-color="#1749FF"/>
                            </no-ssr>
                          </div>
                        </div>
                        <div class="each">
                          <span>نسبت ارزش به قیمت</span>
                          <div>
                            <no-ssr>
                              <star-rating :increment="1" :rating="3" :rtl="true"
                                           :show-rating="false" :star-size="20"
                                           @rating-selected="setRating5" active-color="#1749FF"/>
                            </no-ssr>
                          </div>
                        </div>
                        <div class="each">
                          <span>کیفیت میزبانی</span>
                          <div>
                            <no-ssr>
                              <star-rating :increment="1" :rating="3" :rtl="true"
                                           :show-rating="false" :star-size="20"
                                           @rating-selected="setRating6" active-color="#1749FF"/>
                            </no-ssr>
                          </div>
                        </div>
                      </div>
                      <div class="row">
                        <div class="positive w-50">
                          <div class="input-set">
                            <input placeholder="نقطه قوت" type="text" v-model="proText">
                            <a @click="addPros" href="javascript:void(0)">
                              <i class="bx bx-plus"></i>
                            </a>
                          </div>
                          <div class="items">
                            <ul v-if="commentForm.pros !== ''">
                              <li v-for="pro in prosList" v-if="pro !== ''">
                                <i class="bx bxs-circle"></i>
                                {{ pro }}
                                <span @click="deletePros(pro)"
                                      class="uk-margin-right uk-text-danger">×</span>
                              </li>
                            </ul>
                          </div>
                        </div>
                        <div class="negative w-50">
                          <div class="input-set">
                            <input placeholder="نقطه ضعف" type="text" v-model="conText">
                            <a @click="addCons" href="javascript:void(0)">
                              <i class="bx bx-plus"></i>
                            </a>
                          </div>
                          <div class="items">
                            <ul v-if="commentForm.cons !== ''">
                              <li v-for="con in consList" v-if="con !== ''">
                                <i class="bx bxs-circle"></i>
                                {{ con }}
                                <span @click="deleteCons(con)"
                                      class="uk-margin-right uk-text-danger">×</span>
                              </li>
                            </ul>
                          </div>
                        </div>
                      </div>
                      <div class="row">
                        <textarea placeholder="متن خود را بنویسید" rows="7"
                                  v-model="commentForm.text"></textarea>
                      </div>
                      <div class="row w-100">
                        <a class="cancel uk-modal-close-default" href="javascript:void(0)">بستن</a>
                        <a @click="sendComment" class="submit" href="javascript:void(0)"
                           v-if="sendingComment === false">ارسال</a>
                        <a class="submit" href="javascript:void(0)" v-else>چند لحظه صبر کنید</a>
                      </div>
                    </form>
                  </div>
                </vk-modal>
              </div>
              <div class="totla-comments">
                <div class="each-comment" v-for="comment in residence.comments">
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
                    <p v-html="comment.text"></p>
                    <template v-if="comment.myComment === 1">
                      <br>
                      <a @click="deleteComment" href="javascript:void(0)" style="color: #ff0002;"
                         v-if="deletingComment === false"><small>حذف دیدگاه</small></a>
                      <a href="javascript:void(0)" style="color: #ff0002;" v-else><small>حذف دیدگاه</small></a>
                    </template>
                  </div>
                </div>
              </div>
            </div>

            <div class="point-responsive">
              <h3>{{ residence.crule.title }}</h3>
              <div v-html="residence.crule.text"></div>
            </div>
          </div>

          <aside>
            <no-ssr>
              <div uk-sticky="offset: 100; bottom: true">
                <div class="reserve-request">
                  <div class="night-price">
                    <p>{{ number_3_3(residence.price - residence.offAmount) }} <span>تومان</span>
                    </p>
                    <span>هر شب</span>
                  </div>
                  <form action="">
                    <div class="date">
                      <div class="enter">
                        <i class="bx bx-calendar"></i>
                        <div class="form-group">
                          <label for="enterDate">تاریخ رزرو</label>
                          <input @click="showCalendar" id="enterDate" placeholder="تاریخ ورود - خروج"
                                 type="text"
                                 v-if="date.dates.length === 0"/>
                          <input :placeholder="`از `+date.dates[0].month+`/`+date.dates[0].date+` تا `+date.dates[1].month+`/`+(date.dates[1].date)" @click="showCalendar"
                                 id="enterDate"
                                 type="text" v-else-if="date.dates[0] && date.dates[1]"/>
                          <input @click="showCalendar" id="enterDate" placeholder="انتخاب کنید"
                                 type="text"
                                 v-else/>
                        </div>
                      </div>
                    </div>
                    <div class="count">
                      <a @click="addPerson" class="plus" href="javascript:void(0)"><i
                        class="bx bx-plus"></i></a>
                      <div>
                        <span class="number">{{ person }}</span>
                        <span>تعداد نفرات</span>
                      </div>
                      <a @click="minusPerson" class="minus" href="javascript:void(0)"><i
                        class="bx bx-minus"></i></a>
                    </div>
                    <div class="submit" v-if="$auth.loggedIn">
                      <a @click="confirmInformationModal = true" href="javascript:void(0)"
                         v-if="wait === false">ارسال درخواست رزرو (رایگان)</a>
                      <a href="javascript:void(0)" v-else>چند لحظه صبر کنید</a>
                    </div>
                    <div class="submit" v-else>
                      <a @click="openLogreg" href="javascript:void(0)">ارسال درخواست رزرو (رایگان)</a>
                    </div>
                  </form>
                  <span v-html="calc" v-if="calc !== null"></span>
                  <div class="with" v-html="extra" v-if="extra !== null"></div>
                  <div class="total-price" v-html="total" v-if="total !== null"></div>
                </div>
                <div class="point">
                  <h3>{{ residence.crule.title }}</h3>
                  <div v-html="residence.crule.text"></div>
                </div>
              </div>
            </no-ssr>
          </aside>
        </div>

        <section class="second-category-slider">
          <div class="uk-position-relative">
            <div class="title">
              <div class="name">
                <h3>اقامتگاه‌های مشابه</h3>
              </div>
              <div class="arrows">
              </div>
            </div>
            <div class="uk-slider-container">
              <ul class="uk-slider-items uk-grid">
                <ResidenceLi :key="residence.id" :residence="residence"
                             class="uk-width-1-3 item" v-for="residence in residence.residences"/>
              </ul>
            </div>
          </div>
        </section>
      </section>
    </main>

    <vk-modal :show.sync="confirmInformationModal" center
              class="confirm-information-modal reserve-request-responsive-modal"
              id="confirm-information-modal">
      <div class="iran">
        <div class="title">
          <div>
            <i class="bx bx-layer-plus"></i>
            <h3>تکمیل اطلاعات</h3>
          </div>
          <vk-modal-close @click="confirmInformationModal = false"></vk-modal-close>
        </div>
        <div class="checkbox">
          <label for="foreign-checkbox">اتباع خارجی هستید (? are you foreigner) ؟ کلیک کنید
            !</label>
          <input @change="isForeigner" class="uk-checkbox" id="foreign-checkbox" type="checkbox">
        </div>
        <div class="form">
          <form action="">
            <div class="form-group">
              <label for="name">{{ nameTitle }}</label>
              <input id="name" type="text" v-model="name">
              <span class="error" v-if="errors.name">{{ errors.name[0] }}</span>
            </div>
            <div class="form-group">
              <label for="mellicode">{{ mellicodeTitle }}</label>
              <input id="mellicode" type="text" v-model="mellicode">
              <span class="error" v-if="errors.mellicode">{{ errors.mellicode[0] }}</span>
            </div>
            <div class="form-group">
              <label for="mobile">{{ mobileTitle }}</label>
              <input id="mobile" type="text" v-model="mobile">
              <span class="error" v-if="errors.mobile">{{ errors.mobile[0] }}</span>
            </div>
            <div class="submit">
              <a href="javascript:void(0)" v-if="!$auth.loggedIn" @click="openLogreg">لطفا ابتدا وارد حساب کاربری خود
                شوید</a>
              <button @click="submitOrder" type="button" v-else-if="submitingOrder === false">ثبت
                درخواست
              </button>
              <button type="button" v-else>چند لحظه صبر کنید</button>
            </div>
          </form>
        </div>
      </div>
    </vk-modal>
    <vk-modal :show.sync="reserveRequestResponsiveModal" center class="reserve-request-responsive-modal"
              id="reserve-request-responsive-modal">
      <vk-modal-close @click="reserveRequestResponsiveModal = false"></vk-modal-close>
      <div class="night-price">
        <p>{{ number_3_3(residence.price - residence.offAmount) }} <span>تومان</span></p>
        <span>هر شب</span>
      </div>
      <form action="">
        <div class="date">
          <div class="enter">
            <i class="bx bx-calendar"></i>
            <div class="form-group">
              <label for="enterDater">تاریخ رزرو</label>
              <input @click="showCalendar" id="enterDater" placeholder="انتخاب کنید" type="text"
                     v-if="date.dates.length === 0"/>
              <input :placeholder="`از `+date.dates[0].month+`/`+date.dates[0].date+` تا `+date.dates[1].month+`/`+(date.dates[1].date)" @click="showCalendar"
                     id="enterDater"
                     type="text" v-else-if="date.dates[0] && date.dates[1]"/>
              <input @click="showCalendar" id="enterDater" placeholder="انتخاب کنید" type="text"
                     v-else/>
            </div>
          </div>
        </div>
        <div class="count">
          <a @click="addPerson" class="plus" href="javascript:void(0)"><i
            class="bx bx-plus"></i></a>
          <div>
            <span class="number">{{ person }}</span>
            <span>تعداد نفرات</span>
          </div>
          <a @click="minusPerson" class="minus" href="javascript:void(0)"><i
            class="bx bx-minus"></i></a>
        </div>
        <div class="submit">
          <a @click="confirmInformationModal = true" href="javascript:void(0)"
             v-if="wait === false">ارسال درخواست رزرو (رایگان)</a>
          <a href="javascript:void(0)" v-else>چند لحظه صبر کنید</a>
        </div>
      </form>

      <span v-html="calc" v-if="calc !== null"></span>
      <div class="with" v-html="extra" v-if="extra !== null"></div>
      <div class="total-price" v-html="total" v-if="total !== null"></div>
    </vk-modal>
    <vk-modal :show.sync="noCommentModal" center id="noCommentModal">
      <h5 class="uk-modal-title">ثبت دیدگاه جدید</h5>
      <br><br>
      <p class="uk-text-danger">شما این اقامتگاه را رزرو نکرده‌اید یا هنوز اقامتتان به پایان نرسیده است !
        برای ثبت دیدگاه باید یک بار آن را رزرو کنید و اقامت شما به پایان برسد !</p>
      <br><br>
      <p class="uk-text-right">
        <vk-modal-close @click="noCommentModal = false"></vk-modal-close>
      </p>
    </vk-modal>
    <vk-modal :show.sync="ruleTextModal" center class="share-modal" id="ruleTextModal"
              size="container">
      <div class="modal-content">
        <div class="title">
          <div>
            <img alt="" src="/images/icon-rules.png"/>
            <h3>{{ ruleTitle }}</h3>
          </div>
          <div class="sep"></div>
          <button class="uk-modal-close-default" type="button" uk-close></button>
        </div>
        <br>
        <div class="modal-text">
          <strong class="title">{{ ruleTitle }}</strong>
          <div class="conditions">
            <div class="cond1" v-html="ruleText"></div>
          </div>
        </div>
      </div>
    </vk-modal>
    <vk-modal :show.sync="calendarModal" center class="confirm-information-modal select-province-modal"
              id="calendarModal">
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
            :backward-limit="{ year: residence.previousMonth.year, month: residence.previousMonth.month }"
            :forward-limit="{ year: residence.nextMonth.year, month: residence.nextMonth.month }"
            :days="residence.calendars"
            :debug-selector="false"
            :defPrice="residence.priceCalendar"
            :disabled-map="{}"
            :enable-all-days="true"
            :holiday-map="{}"
            :holidayPrice="residence.priceVacationCalendar"
            @datemodel="handledate"
            lang="Jalali"
            size="md"
            type="range"
          />
        </form>
      </div>
    </vk-modal>
    <vk-modal :show.sync="shareModal" center class="share-modal" id="share-modal">
      <div class="modal-content">
        <div class="title">
          <div>
            <i class="bx bx-layer-plus"></i>
            <h3>اشتراک گذاری</h3>
          </div>
          <vk-modal-close @click="shareModal = false"></vk-modal-close>
        </div>
        <div class="all-socials">
          <a
            :href="`https://twitter.com/intent/tweet?url=https://boroberim.net`+this.$route.path+`&text=`+residence.title">
            <span>twitter</span>
            <i class="bx bxl-twitter"></i>
          </a>
          <a :href="`https://api.whatsapp.com/send?text=https://boroberim.net`+this.$route.path">
            <span>whatsapp</span>
            <i class="bx bxl-whatsapp"></i>
          </a>
          <a
            :href="`http://www.linkedin.com/shareArticle?mini=true&url=https://boroberim.net`+this.$route.path+`&title=&summary=`+residence.title">
            <span>linkedin</span>
            <i class="bx bxl-linkedin"></i>
          </a>
          <a
            :href="`https://telegram.me/share/url?url=https://boroberim.net`+this.$route.path+`&text=`+residence.title">
            <span>telegram</span>
            <i class="bx bxl-telegram"></i>
          </a>
        </div>
      </div>
    </vk-modal>
  </div>
</template>

<style scoped>
  .aboutText {
    color: black;
    line-height: 35px;
    text-align: justify;
  }

  #enterDate {
    height: auto;
    padding: 0 !important;
    margin: 0 !important;
    background-color: transparent;
    border: 0;
  }

  .flex-rules {
    display: flex;
    flex-direction: row;
    align-items: center;
    margin: 40px 0px 0px 0px;
  }

  .flex-rules > div {
    flex: 1;
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    position: relative;
  }

  .flex-rules > div > strong,
  .flex-rules > div > img {
    margin-bottom: 37px;
    display: block;
  }

  .flex-rules + .points {
    margin-top: 50px;
  }

  .points p > img {
    margin-left: 10px;
  }

  .points p {
    color: #2657ff !important;
    cursor: pointer;
  }

  .pointsBlack p {
    color: #000000 !important;
    cursor: auto;
  }

  .share-modal .uk-modal-body .title > div {
    flex-direction: column;
    width: 100%;
  }

  .share-modal .uk-modal-body .title > div > img {
    margin-bottom: 15px;
    margin-top: 15px;
  }

  .sep {
    width: 100%;
    height: 20px;
    background: url("/images/sep.png");
    background-size: 100% 100%;
  }

  .share-modal .uk-modal-body .title {
    flex-direction: column;
  }

  .share-modal .uk-modal-body .title button {
    display: none !important;
  }

  .modal-text {
    padding: 0 90px;
  }

  .modal-text .title {
    margin-bottom: 20px;
    text-align: right;
    align-items: flex-start !important;
    display: block !important;
    position: relative;
  }

  .modal-text > p {
    line-height: 35px;
    font-size: 13px;
  }

  .modal-text .title::after {
    content: "";
    width: 51px;
    height: 4px;
    background: #416bff;
    position: absolute;
    top: 22px;
    right: 0;
  }

  .conditions {
    margin-top: 20px;
  }

  .conditions > div > div {
    display: flex;
    align-items: center;
  }

  .conditions > div > div > img {
    margin-right: -24px;
    margin-left: 5px;
  }

  .conditions ul li {
    font-size: 13px;
    line-height: 31px;
    margin-top: -10px;
  }

  .flex-rules > div::before {
    content: "";
    width: 1px;
    height: 20px;
    border-left: 1px dashed #d6d6d6;
    position: absolute;
    left: 50%;
    top: 25px;
    transform: translateX(-50%);
  }

  .flex-rules > div::after {
    content: "";
    width: 1px;
    height: 20px;
    border-left: 1px dashed #d6d6d6;
    position: absolute;
    left: 50%;
    bottom: 25px;
    transform: translateX(-50%);
  }
</style>

<script>
  import datepicker from '@/components/datepicker1'
  import ResidenceLi from '~/components/ResidenceLi'
  import { mapGetters } from 'vuex'

  export default {
    components: {
      datepicker,
      ResidenceLi
    },
    data() {
      return {
        confirmInformationModal: false,
        reserveRequestResponsiveModal: false,
        noCommentModal: false,
        ruleTextModal: false,
        calendarModal: false,
        shareModal: false,
        nearbyPlaceModal: false,
        parkingModal: false,
        roomModal: false,
        facilityModal: false,
        roadOptionModal: false,
        enviromentalOptionModal: false,
        wcModal: false,
        commentModal: false,
        mobileView: 0,
        id: 0,
        foreigner: 2,
        nameTitle: 'نام و نام خانوادگی',
        mellicodeTitle: 'کد ملی',
        mobileTitle: 'شماره موبایل',
        name: null,
        mellicode: null,
        mobile: null,
        residence: [],
        amount: 0,
        person: 1,
        days: 0,
        reserveCalculator: null,
        date: {
          dates: []
        },
        showModal: false,
        calc: null,
        extra: null,
        total: null,
        wait: false,
        submitingOrder: false,
        sendingComment: false,
        deletingComment: false,
        proText: null,
        conText: null,
        pros: '',
        cons: '',
        commentForm: {
          id: 0,
          score1: 3,
          score2: 3,
          score3: 3,
          score4: 3,
          score5: 3,
          score6: 3,
          text: null,
          pros: '',
          cons: ''
        },
        nearbyPlace: [],
        parking: [],
        room: [],
        wc: [],
        ruleText: null,
        ruleTitle: null,
        errors: []
      }
    },
    async asyncData({ params, $axios, redirect }) {
      let { data } = await $axios.post('api/v1/showResidence', { 'id': params.residence, 'city':params.city,'province':params.province,'type':params.type })
      if (data.status === 0)
        redirect('/404');
      return {
        id: params.residence,
        commentForm: {
          id: params.residence,
          score1: 3,
          score2: 3,
          score3: 3,
          score4: 3,
          score5: 3,
          score6: 3,
          text: null,
          pros: '',
          cons: ''
        },
        residence: data.data
      }
    },
    methods: {
      async addToFavorite(id) {
        let res = await this.$axios.post('api/v1/user/addToFavorite', { id: this.residence.id });
        if (res.data.status === 1) {
          if (this.residence.favorite === 0)
            this.residence.favorite = 1;
          else
            this.residence.favorite = 0;
          this.$snotify.success(res.data.msg, {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          })
        } else if (res.data.status === 422) {
          this.errors = res.data.errors;
          this.$snotify.warning(res.data.msg, {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          })
        } else {
          this.$snotify.error(res.data.msg, {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          })
        }
      },
      isForeigner() {
        if (this.foreigner === 2) {
          this.foreigner = 1;
          this.nameTitle = 'name';
          this.mellicodeTitle = 'passport number';
          this.mobileTitle = 'mobile';
        } else {
          this.foreigner = 2;
          this.nameTitle = 'نام و نام خانوادگی';
          this.mellicodeTitle = 'کد ملی';
          this.mobileTitle = 'شماره موبایل';
        }
      },
      showCalendar() {
        this.calendarModal = true;
      },
      handledate(e) {
        this.date = e;
      },
      addPerson() {
        if (this.person < this.residence.detail.maxPerson) {
          if (this.wait === false) {
            this.person++;
            this.calc = null;
            this.extra = null;
            this.total = null;
            if (this.date.dates.length === 2)
              this.getPrice();
          }
        } else
          this.$snotify.error('تعداد درخواستی از حداکثر ظرفیت اقامتگاه بیشتر است', {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          })
      },
      minusPerson() {
        if (this.person > 1) {
          if (this.wait === false) {
            this.person--;
            this.calc = null;
            this.extra = null;
            this.total = null;
            if (this.date.dates.length === 2)
              this.getPrice();
          }
        }
      },
      async getPrice() {
        if(this.mobileView === 1)
          this.reserveRequestResponsiveModal = true;
        this.calendarModal = false;
        this.wait = true;
        const { data } = await this.$axios.post(`api/v1/checkDate`, {
          date: this.date.dates,
          id: this.residence.id,
          person: this.person
        });
        if (data.status === 1) {
          this.calc = data.calc;
          this.extra = data.extra;
          this.total = data.total;
        } else if (data.status === 4 || data.status === 5) {
          this.$snotify.error(data.msg, {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          });
          this.$router.push('/residence/' + this.residence.type + '/' + this.residence.province + '/' + this.residence.city + '' + this.residence.id)
        } else
          this.$snotify.error(data.msg, {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          });
        this.wait = false;
      },
      async submitOrder() {
        this.errors = [];
        if (this.date.dates.length === 2) {
          this.submitingOrder = true;
          const order = await this.$axios.post(`api/v1/user/submitOrder`, {
            date: this.date.dates,
            id: this.residence.id,
            person: this.person,
            name: this.name,
            mellicode: this.mellicode,
            mobile: this.mobile,
            foreigner: this.foreigner
          });
          if (order.data.status === 1) {
            this.confirmInformationModal = false;
            this.$snotify.success(order.data.msg, {
              timeout: 2000,
              showProgressBar: false,
              closeOnClick: true,
              pauseOnHover: true
            });
            this.$router.push('/checkOut')
          } else if (order.data.status === 422) {
            this.errors = order.data.errors;
            this.$snotify.warning(order.data.msg, {
              timeout: 2000,
              showProgressBar: false,
              closeOnClick: true,
              pauseOnHover: true
            })
          } else {
            this.$snotify.error(order.data.msg, {
              timeout: 2000,
              showProgressBar: false,
              closeOnClick: true,
              pauseOnHover: true
            })
          }
          this.submitingOrder = false
        } else {
          this.$snotify.error('لطفاً ابتدا چند روز را از تقویم انتخاب کنید', {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          })
        }
      },
      async sendComment() {
        this.errors = [];
        this.sendingComment = true;
        const comment = await this.$axios.post(`api/v1/user/sendComment`, this.commentForm)
        if (comment.data.status === 1) {
          this.commentModal = false;
          this.$snotify.success(comment.data.msg, {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          });

          let showResidence = await this.$axios.post('api/v1/showResidence', {
            'type': this.residence.type,
            'provine': this.residence.province,
            'city': this.residence.city,
            'id': this.residence.id
          });
          this.commentForm.pros = '';
          this.commentForm.cons = '';
          this.commentForm.text = null;
          this.residence = showResidence.data.data
        } else if (comment.data.status === 422) {
          this.errors = comment.data.errors;
          this.$snotify.warning(comment.data.msg, {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          })
        } else {
          this.$snotify.error(comment.data.msg, {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          })
        }
        this.sendingComment = false;
      },
      async deleteComment() {
        this.deletingComment = true;
        const deleteComment = await this.$axios.post(`api/v1/user/deleteComment`, { 'id': this.residence.id })
        if (deleteComment.data.status === 1) {
          this.$snotify.success(deleteComment.data.msg, {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          });

          let showResidence = await this.$axios.post('api/v1/showResidence', {
            'type': this.residence.type,
            'provine': this.residence.province,
            'city': this.residence.city,
            'id': this.residence.id
          });
          this.commentForm.pros = '';
          this.commentForm.cons = '';
          this.commentForm.text = null;
          this.residence = showResidence.data.data
        } else if (deleteComment.data.status === 422) {
          this.errors = deleteComment.data.errors;
          this.$snotify.warning(deleteComment.data.msg, {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          })
        } else {
          this.$snotify.error(deleteComment.data.msg, {
            timeout: 2000,
            showProgressBar: false,
            closeOnClick: true,
            pauseOnHover: true
          })
        }
        this.deletingComment = false
      },
      setNearbyPlace(nearbyPlace) {
        this.nearbyPlace = nearbyPlace;
        this.nearbyPlaceModal = true;
      },
      setParking(parking) {
        this.parking = parking;
        this.parkingModal = true;
      },
      setRoom(room) {
        this.room = room;
        this.roomModal = true;
      },
      setWc(wc) {
        this.wc = wc;
        this.wcModal = true;
      },
      setRating1(score) {
        this.commentForm.score1 = score;
      },
      setRating2(score) {
        this.commentForm.score2 = score;
      },
      setRating3(score) {
        this.commentForm.score3 = score;
      },
      setRating4(score) {
        this.commentForm.score4 = score;
      },
      setRating5(score) {
        this.commentForm.score5 = score;
      },
      setRating6(score) {
        this.commentForm.score6 = score;
      },
      addPros() {
        if (this.proText !== null) {
          this.commentForm.pros += this.proText + '|';
          this.proText = null;
        }
      },
      addCons() {
        if (this.conText !== null) {
          this.commentForm.cons += this.conText + '|';
          this.conText = null;
        }
      },
      deletePros(pro) {
        let ar = this.commentForm.pros.split('|');
        let str = '';
        for (const element of ar) {
          if (pro !== element && element !== '')
            str += element + '|';
        }
        this.commentForm.pros = str;
      },
      deleteCons(con) {
        let ar = this.commentForm.cons.split('|');
        let str = '';
        for (const element of ar) {
          if (con !== element && element !== '')
            str += element + '|';
        }
        this.commentForm.cons = str
      },
      showRule(title, text) {
        this.ruleTitle = title;
        this.ruleText = text;
        this.ruleTextModal = true;
      },
      showReserveRequestResponsiveModal() {
        this.mobileView = 1;
        this.reserveRequestResponsiveModal = true;
      },
      number_3_3(num, sep) {
        const number = typeof num === 'number' ? num.toString() : num;
        const separator = typeof sep === 'undefined' ? ',' : sep;
        return number.replace(/(\d)(?=(\d\d\d)+(?!\d))/g, '$1' + separator);
      },
      openLogreg() {
        this.confirmInformationModal = false;
        if(window.innerWidth < 700) {
          this.$store.dispatch('setVuexMenuState',false);
          if(this.vuexLogregMobileState === true) {
            this.$store.dispatch('setVuexLogregMobileState',false);
          } else {
            this.$store.dispatch('setVuexLogregMobileState',true);
          }
        } else {
          if(this.vuexLogregState === true) {
            this.$store.dispatch('setVuexLogregState',false);
          } else {
            this.$store.dispatch('setVuexLogregState',true);
          }
        }
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
      dateObj() {
        return this.date?.dates;
      },
      consList() {
        return this.commentForm.cons.split('|');
      },
      prosList() {
        return this.commentForm.pros.split('|');
      },
      vuexLogregState:
      {
        get() { return this.$store.getters.vuexLogregState },
        set() { this.$store.dispatch('setVuexLogregState',false); }
      },
      vuexMenuState() {
        return this.$store.getters.vuexMenuState
      },
      vuexLogregMobileState() {
        return this.$store.getters.vuexLogregMobileState
      },
      ...mapGetters(['settings'])
    },
    mounted() {
      if (!process.server && !window.jQuery) {
        const script = document.createElement('script');
        script.onload = this.onScriptLoaded;
        script.type = 'text/javascript';
        script.src = '/plugins/uikit-3.6.22/js/uikit.min.js';
        document.head.appendChild(script);
      }
    },
    watch: {
      date: {
        deep: true,
        handler(e) {
          if (e.dates.length === 2) {
            this.showModal = false;
            this.getPrice();
          }
        }
      }
    },
    created() {
      this.$store.dispatch('setVuexMenuState',false);
    },
    head() {
      return {
        title: 'نمایش اقامتگاه - ' + this.residence.title,
        meta: [
          {
            hid: 'description',
            name: 'description',
            content: this.residence.detail.metaDesc
          }
        ]
      }
    }
  }
</script>
