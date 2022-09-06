<template>
  <div>
    <main ref="main" class="main" :class="[vuexMenuState ? 'blur' : '',vuexLogregMobileState ? 'blur' : '']" @click="closeMenu">
      <section class="my-trips">
        <div class="title">
          <h1>سفرهای من</h1>
          <hr>
          <div>
            <a href="javascript:void(0)" v-bind:class="[status === null ? 'active' : '']" @click="changeStatus(null)">پیش رو</a>
            <a href="javascript:void(0)" v-bind:class="[status === 1 ? 'active' : '']" @click="changeStatus(1)">پرداخت شده</a>
            <a href="javascript:void(0)" v-bind:class="[status === 2 ? 'active' : '']" @click="changeStatus(2)">کنسل شده</a>
            <a href="javascript:void(0)" v-bind:class="[status === 3 ? 'active' : '']" @click="changeStatus(3)">شکایت شده</a>
            <a href="javascript:void(0)" v-bind:class="[status === 4 ? 'active' : '']" @click="changeStatus(4)">شروع شده</a>
            <a href="javascript:void(0)" v-bind:class="[status === 5 ? 'active' : '']" @click="changeStatus(5)">تمام شده</a>
          </div>
        </div>
        <div class="items">
          <div class="item" v-for="orderResidence in orderResidences" :key="orderResidence.id">
            <div class="right">
              <div class="img">
                <nuxt-link :to="`/`+orderResidence.residence.typeSlug+'/'+orderResidence.residence.provinceSlug+'/'+orderResidence.residence.citySlug+'/'+orderResidence.residence.id">
                  <img :src="orderResidence.image" :alt="orderResidence.residence.title">
                </nuxt-link>
              </div>
              <div class="infos">
                <!-- place name -->
                <h3>
                  <nuxt-link :to="`/`+orderResidence.residence.typeSlug+'/'+orderResidence.residence.provinceSlug+'/'+orderResidence.residence.citySlug+'/'+orderResidence.residence.id">
                    {{ orderResidence.residence.title }}
                  </nuxt-link>
                </h3>
                <!-- place location -->
                <div class="locate">
                  <i class="bx bx-current-location"></i>
                  <p>
                    <nuxt-link :to="`/`+orderResidence.residence.typeSlug+'/'+orderResidence.residence.provinceSlug+'/'+orderResidence.residence.citySlug">{{ orderResidence.residence.city }}</nuxt-link>
                    , <nuxt-link :to="`/`+orderResidence.residence.typeSlug+'/'+orderResidence.residence.provinceSlug">{{ orderResidence.residence.province }}</nuxt-link>
                  </p>
                </div>
                <!-- place rate -->
                <div class="rate">
                  <span class="icon"><i class="bx bxs-star"></i></span>
                  <span>{{ orderResidence.residence.score }}</span>
                </div>
                <!-- palce plain text -->
                <div class="text">
                  <p class="red">
                    <i class="bx bx-notification"></i>
                    <template v-if="orderResidence.ownerStatus === 2">
                      <span>درخواست رزرو شما توسط میزبان تایید نشد</span>
                    </template>
                    <template v-else>
                      <span v-show="orderResidence.status === 1">
                        <template v-if="orderResidence.ownerStatus === 1">
                          درخواست رزرو شما در انتظار بررسی و تایید میزبان می‌باشد
                        </template>
                        <template v-else-if="orderResidence.ownerStatus === 3">
                          درخوست رزرو توسط میزبان تأیید شده است
                        </template>
                      </span>
                      <span v-show="orderResidence.status === 2">پرداخت با موفقیت انجام شده است</span>
                      <span v-show="orderResidence.status === 3">پرداخت هزینه توسط شما کنسل شده یا به درستی انجام نشده است</span>
                      <span v-show="orderResidence.status === 4">خطا در پرداخت آنلاین</span>
                      <span v-show="orderResidence.status === 5">این درخواست رزرو توسط شما کنسل شده است</span>
                      <span v-show="orderResidence.status === 6">این درخواست رزرو توسط بروبریم کنسل شده است</span>
                      <span v-show="orderResidence.status === 7">این درخواست رزرو توسط میزبان اقامتگاه کنسل شده است</span>
                      <span v-show="orderResidence.status === 8">کلید اقامتگاه آماده تحویل به شما می باشد</span>
                      <span v-show="orderResidence.status === 9">اقامتگاه به شما تحویل داده شده است</span>
                      <span v-show="orderResidence.status === 10">این رزرو در پروسه بررسی شکایت مهمان قرار دارد</span>
                      <span v-show="orderResidence.status === 11">تحویل اقامتگاه به میزبان توسط شما</span>
                      <span v-show="orderResidence.status === 12">تأیید اتمام اقامت توسط </span>
                    </template>
                  </p>
                  <p class="blue">
                    <i class="bx bx-notification"></i>
                    <template v-if="orderResidence.status === 1 || orderResidence.status === 3 || orderResidence.status === 4">
                      <span v-if="orderResidence.ownerStatus === 3 && orderResidence.hourRemaining !== -1 && orderResidence.minuteRemaining !== -1">
                        در انتظار پرداخت
                      </span>
                      <span v-else-if="orderResidence.ownerStatus === 3 && orderResidence.hourRemaining === -1 && orderResidence.minuteRemaining === -1">
                        پرداخت هزینه توسط شما انجام نشد
                      </span>
                      <span v-else-if="orderResidence.ownerStatus === 2">
                        درخواست شما توسط میزبان رد شد
                      </span>
                      <span v-else>
                        در انتظار تأیید میزبان
                      </span>
                    </template>
                    <span v-else-if="orderResidence.status === 5 || orderResidence.status === 6 || orderResidence.status === 7">لغو درخواست رزرو</span>
                    <span v-else>تأیید درخواست توسط میزبان</span>
                  </p>
                </div>
                <!-- place host -->
                <div class="text" v-if="orderResidence.ownerStatus === 3 && orderResidence.status === 2 || orderResidence.status === 8 || orderResidence.status === 9 || orderResidence.status === 11 || orderResidence.status === 12">
                  <p class="blue">
                    <i class="bx bx-current-location"></i>
                    {{ orderResidence.residence.address }}
                  </p>
                </div>
                <div class="host" v-if="orderResidence.ownerStatus === 3 && orderResidence.status === 2 || orderResidence.status === 8 || orderResidence.status === 9 || orderResidence.status === 11 || orderResidence.status === 12">
                  <img :src="orderResidence.imageUser" alt="تصویر میزبان">
                  <p>{{ orderResidence.residence.user }}</p>
                  <p>{{ orderResidence.residence.hostMobile }}</p>
                  <a href="#">
                    <i class="bx bx-chat"></i>
                    گفتگو با میزبان
                  </a>
                </div>
                <!-- Timer -->
                <template v-if="orderResidence.ownerStatus === 3 && orderResidence.status !== 5 && orderResidence.status !== 6
                && orderResidence.status !== 7 && orderResidence.status !== 10">
                  <Counter :hour="orderResidence.hourRemaining" :minute="orderResidence.minuteRemaining"
                           v-if="orderResidence.ownerStatus === 3 && orderResidence.status === 1 || orderResidence.status === 3 || orderResidence.status === 4" />
                </template>
                <!-- reserve place date -->
                <div class="date">
                  <div class="enter">
                    <i class="bx bx-calendar"></i>
                    <div class="form-group">
                      <label>تاریخ ورود</label>
                      <input type="text" disabled :value="orderResidence.startDate">
                    </div>
                  </div>

                  <div class="exit">
                    <i class="bx bx-calendar"></i>
                    <div class="form-group">
                      <label>تاریخ خروج</label>
                      <input type="text" disabled :value="orderResidence.endDate">
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="left">
              <div class="night-price-discount">
                <div class="discount">
                  <p style="text-decoration:none;">{{ number_3_3(orderResidence.amount) }}</p>
                  <span v-show="orderResidence.off !== 0">{{ orderResidence.off }}% تخفیف</span>
                </div>
                <div class="night-price">
                  <p>{{ number_3_3(orderResidence.priceDefault) }} تومان</p>
                  <span>هر شب</span>
                </div>
              </div>
              <div class="price">
                <div class="with">
                  <p>شماره سفارش :</p>
                  <p>#{{ orderResidence.id }}</p>
                </div>
                <div class="calc" v-for="groupeDate in orderResidence.groupeDates">
                  <div>
                    <span>{{ groupeDate.total }} شب</span>
                    <span class="stroke" uk-close></span>
                    <span>{{ number_3_3(groupeDate.price) }} تومانی</span>
                  </div>
                  <p>{{ number_3_3(groupeDate.total*groupeDate.price) }} تومان</p>
                </div>
                <div class="with" v-show="orderResidence.offId !== 0">
                  <p>همراه با کد تخفیف</p>
                  <p>{{ number_3_3(orderResidence.offAmount) }} تومانی</p>
                </div>
                <div class="with" v-show="orderResidence.extraPerson !== 0">
                  <p>{{ orderResidence.extraPerson }} نفر همراه</p>
                  <p>{{ number_3_3(orderResidence.extraPersonAmount) }} تومان</p>
                </div>
                <div class="total-price">
                  <span>جمع کل : </span>
                  <p>{{ number_3_3(orderResidence.amount) }} تومان</p>
                </div>
              </div>
              <div class="submit" v-if="orderResidence.status === 1 || orderResidence.status === 3 || orderResidence.status === 4">
                <a href="javascript:void(0)" class="continue" v-if="orderResidence.ownerStatus === 3 && orderResidence.hourRemaining !== -1 && orderResidence.minuteRemaining !== -1" @click="setOrder(orderResidence)">
                  ادامه و پرداخت
                </a>
                <p v-else-if="orderResidence.ownerStatus === 3 && orderResidence.hourRemaining === -1 && orderResidence.minuteRemaining === -1"></p>
                <a href="javascript:void(0)" class="continue" v-else-if="orderResidence.ownerStatus === 2">رد درخواست رزرو توسط میزبان</a>
                <a href="javascript:void(0)" class="continue" v-else>در انتظار تأیید میزبان</a>
              </div>
              <template v-if="orderResidence.ownerStatus !== 2 && orderResidence.ended === 2">
                <div class="submit" v-if="orderResidence.status === 1 || orderResidence.status === 2 || orderResidence.status === 3
                || orderResidence.status === 4 || orderResidence.status === 8 || orderResidence.status === 9">
                  <a href="javascript:void(0)" class="cancel" v-if="canceling === false" @click="cancelOrder(orderResidence.id)">کنسل کردن</a>
                  <a href="javascript:void(0)" class="cancel" v-else>چند لحظه صبر کنید</a>
                </div>
              </template>
              <template v-if="orderResidence.started === 1">
                <div class="submit" v-if="orderResidence.status === 2 && orderResidence.ownerStatus === 3">
                  <a href="javascript:void(0)" class="continue" v-if="changingStatus === false" @click="changeOrderStatus(orderResidence.id,9)">کلید را تحویل گرفتم</a>
                  <a href="javascript:void(0)" class="continue" v-else>چند لحظه صبر کنید</a>
                </div>
                <div class="submit" v-if="orderResidence.status === 9 && orderResidence.ownerStatus === 3">
                  <a href="javascript:void(0)" class="continue" v-if="changingStatus === false" @click="changeOrderStatus(orderResidence.id,11)">تحویل اقامتگاه</a>
                  <a href="javascript:void(0)" class="continue" v-else>چند لحظه صبر کنید</a>
                </div>
              </template>
              <div class="submit" v-if="orderResidence.status === 9 && orderResidence.ownerStatus === 3">
                <a href="javascript:void(0)" class="cancel" @click="startComplain(orderResidence.id)">ثبت شکایت</a>
              </div>
              <div class="submit" v-if="orderResidence.status === 11 || orderResidence.status === 12">
                <nuxt-link :to="`/residence/`+orderResidence.residence.id+`/`+orderResidence.residence.slug+`#comments`" class="cancel">ثبت امتیاز</nuxt-link>
              </div>
              <template v-if="orderResidence.ended === 2">
                <div class="submit" v-if="orderResidence.status !== 5 && orderResidence.status !== 6 && orderResidence.status !== 7 && orderResidence.ownerStatus === 3">
                  <a href="javascript:void(0)" class="continue" @click="showInsuranceModal(orderResidence.insurances,orderResidence.id,orderResidence.person,orderResidence.residence.id)">ثبت درخواست بیمه</a>
                </div>
              </template>
              <template v-if="orderResidence.insuranceFile !== null">
                <div class="submit">
                  <a :href="orderResidence.insuranceFile" class="continue">دانلود بیمه نامه</a>
                </div>
              </template>
            </div>
          </div>
        </div>
      </section>
    </main>
    <section class="factor" :class="[vuexChackOutMenuState ? 'active' : '']">
      <button class="close" @click="closeOrder">×</button>
      <h2>
        <nuxt-link :to="`/residence/`+order.residence.id+`/`+order.residence.slug">
          {{ order.residence.title }}
        </nuxt-link>
      </h2>
      <nuxt-link :to="`/residence/`+order.residence.id+`/`+order.residence.slug">
        <img :src="order.image" :alt="order.residence.title">
      </nuxt-link>
      <div class="code">
        <div class="right">
          <span>شناسه رزرو :</span>
          <p>#{{ order.id }}</p>
        </div>
        <div class="left">
          <template v-if="order.status === 1 || order.status === 3 || order.status === 4">
            <span v-if="order.ownerStatus === 3 && order.hourRemaining !== -1 && order.minuteRemaining !== -1">
              در انتظار پرداخت
            </span>
            <span v-else-if="order.ownerStatus === 3 && order.hourRemaining === -1 && order.minuteRemaining === -1">
              پرداخت توسط شما انجام نشد
            </span>
            <span v-else-if="order.ownerStatus === 2">
              درخواست شما توسط میزبان رد شد
            </span>
            <span v-else>
              در انتظار تأیید میزبان
            </span>
          </template>
          <span v-else-if="order.status === 5 || order.status === 6 || order.status === 7">لغو درخواست رزرو</span>
          <span v-else>تأیید درخواست توسط میزبان</span>
        </div>
      </div>
      <div class="date">
        <div class="enter">
          <i class="bx bx-calendar"></i>
          <div class="form-group">
            <label>تاریخ ورود</label>
            <input type="text" disabled :value="order.startDate">
          </div>
        </div>
        <div class="exit">
          <i class="bx bx-calendar"></i>
          <div class="form-group">
            <label>تاریخ خروج</label>
            <input type="text" disabled :value="order.endDate">
          </div>
        </div>
      </div>
      <div class="price-discount">
        <div class="night-price-discount">
          <div class="discount">
            <p style="text-decoration:none;">{{ number_3_3(order.amount) }}</p>
            <span v-show="order.off !== 0">{{ order.off }}% تخفیف</span>
          </div>
          <div class="night-price">
            <p>{{ number_3_3(order.priceDefault) }} تومان</p>
            <span>هر شب</span>
          </div>
        </div>
        <div class="price">
          <div class="calc" v-for="groupeDate in order.groupeDates">
            <div>
              <span>{{ groupeDate.total }} شب</span>
              <span class="stroke" uk-close></span>
              <span>{{ number_3_3(groupeDate.price) }} تومانی</span>
            </div>
            <p>{{ number_3_3(groupeDate.total*groupeDate.price) }} تومان</p>
          </div>
          <div class="with" v-show="order.offId !== 0">
            <p>همراه با کد تخفیف</p>
            <p>{{ number_3_3(order.offAmount) }} تومانی</p>
          </div>
          <div class="with" v-show="order.extraPerson !== 0">
            <p>{{ order.extraPerson }} نفر همراه</p>
            <p>{{ number_3_3(order.extraPersonAmount) }} تومان</p>
          </div>
          <div class="total-price">
            <span>جمع کل : </span>
            <p>{{ number_3_3(order.amount) }} تومان</p>
          </div>
        </div>
        <form action="">
          <label for="offCode">کد تخفیف</label>
          <div>
            <input type="text" id="offCode" v-model="off" placeholder="کد تخفیف را وارد نمایید">
            <button type="button" v-if="checkingOffCode === false" @click="applyOffCode">اعمال</button>
            <button type="button" v-else>در حال بررسی</button>
          </div>
        </form>
      </div>
      <div class="submit" v-if="order.hourRemaining !== -1 && order.minuteRemaining !== -1">
        <a :href="url+`/pay/`+order.id">پرداخت</a>
      </div>
      <div class="submit" v-if="order.hourRemaining !== -1 && order.minuteRemaining !== -1">
        <a href="javascript:void(0)" @click="payWithWallet(order.id)" v-if="payingWithWallet === false">پرداخت با کیف پول</a>
        <a href="javascript:void(0)" v-else>چند لحظه صبر کنید</a>
      </div>
      <div class="submit">
        <a href="javascript:void(0)" class="cancel" @click="closeOrder">بستن</a>
      </div>
    </section>
    <vk-modal center id="commentModal" class="send-comment-modal uk-modal-container" :show.sync="complainModal">
      <h3 class="uk-modal-title">ثبت شکایت</h3>
      <div class="text">
        <form action="#">
          <p>مشکلی پیش آمده ؟ مشکل خود را مطرح کنید تا ما رسیدگی کنیم</p><br>
          <div class="row">
            <textarea rows="7" placeholder="موضوع شکایت" v-model="complainSubject"></textarea>
            <span v-if="errors.complainSubject" class="error">{{ errors.complainSubject[0] }}</span>
          </div>
          <div class="row">
            <textarea rows="7" placeholder="شکایت خود را بنویسید" v-model="complainText"></textarea>
            <span v-if="errors.complainText" class="error">{{ errors.complainText[0] }}</span>
          </div>
          <div class="row w-100">
            <a href="javascript:void(0)" class="cancel uk-modal-close-default" @click="complainModal = false">بستن</a>
            <a href="javascript:void(0)" class="submit" @click="sendComplain" v-if="complaining === false">
              ثبت شکایت
            </a>
            <a href="javascript:void(0)" class="submit" v-else>چند لحظه صبر کنید</a>
          </div>
        </form>
      </div>
    </vk-modal>
    <vk-modal center size="container" id="insuranceModal" class="modal-info-submit" :show.sync="insuranceModal">
      <div class="title">
        <p>
          <i class="bx bx-doughnut-chart"></i>
          خود و همراهانتان را بیمه کنید
        </p>
        <vk-modal-close @click="insuranceModal = false"></vk-modal-close>
      </div>
      <div class="text">
        <div class="row">
          <div class="form-group w-30">
            <input id="nameAndFamily" type="text" placeholder="" v-model="newInsurance.name" required>
            <span class="floating-label">نام و نام خانوادگی</span>
          </div>
          <div class="form-group w-30">
            <input id="melliCode" type="text" placeholder="" v-model="newInsurance.codeMelli" required>
            <span class="floating-label">کد ملی</span>
          </div>
          <div class="form-group w-30">
            <input id="birthDate" type="text" placeholder="" v-model="newInsurance.birthDate" required>
            <span class="floating-label">تاریخ تولد</span>
          </div>
        </div>

        <div class="submit-cancel">
          <button type="button" class="uk-button uk-button-primary w-100" @click="addInsurance">
            اضافه کن
          </button>
        </div>

        <div class="submit-cancel">
          <table class="table">
            <thead>
              <tr>
                <th>نام و نام خانوادگی</th>
                <th>کدملی</th>
                <th>تاریخ تولد</th>
                <th>عملیات</th>
              </tr>
            </thead>
            <tbody>
            <tr v-for="(insurance, index) in insurances" v-if="insurance.name !== null">
              <td>{{ insurance.name }}</td>
              <td>{{ insurance.code_melli }}</td>
              <td>{{ insurance.birth_date }}</td>
              <td>
                <button @click="deleteInsurance(index)">
                  <span class="remove-lines"></span>
                  <span>حذف</span>
                </button>
              </td>
            </tr>
            </tbody>
          </table>
        </div>

        <div class="submit-cancel">
          <button type="button" class="uk-button uk-button-primary w-100" @click="showPrevInsurancesModal">
            انتخاب از لیست
          </button>
        </div>

        <div class="submit-cancel">
          <button type="button" class="submit" v-if="sendingInsurance === false" @click="sendInsurance">
            <i class="bx bx-chevron-right"></i>
            ارسال
          </button>
          <button class="submit" v-else>
            چند لحظه صبر کنید
          </button>
        </div>
      </div>
    </vk-modal>
    <vk-modal center id="prevInsuranceModal" class="modal-info-submit" :show.sync="prevInsurancesModal">
      <div class="title">
        <p>
          <i class="bx bx-doughnut-chart"></i>
          لیست مسافر های همراه شما
        </p>
        <vk-modal-close @click="prevInsurancesModal = false"></vk-modal-close>
      </div>
      <div class="text">
        <div class="uk-margin-bottom" v-for="prevInsurance in prevInsurances">
          <button type="button" class="uk-button uk-button-default w-100" @click="chooseInsurance(prevInsurance)">
            {{ prevInsurance.name }} {{ prevInsurance.codeMelli }}
          </button>
        </div>
      </div>
    </vk-modal>
  </div>
</template>

<style scoped>
  * {
    box-sizing: inherit !important;
  }
  .table,
  .table thead,
  .table tbody {
    width: 100%;
  }
  .table td,
  .table th {
    width: 25%; border-bottom: 1px solid #eee !important; text-align: center; padding: 15px 0;
  }
  .table th {
    width: 25%;
  }
  .table td {
    width: 25%;
  }
  .remove-lines {
    width: 5px; height: 5px; position: relative;
  }
  .remove-lines::after {
    content: ""; width: 1px; height: 65%; background: red; position: absolute; top: 17px; left: 0px; transform: rotate(135deg) translateY(5px);
    transform-origin: top left;
  }
  .remove-lines::before {
    content: ""; width: 1px; height: 65%; background: red; position: absolute; top: 0px; right: 0px; transform: rotate(45deg) translateY(5px);
    transform-origin: top left;
  }
  .remove-lines + span {
    padding-right: 18px; color: red;
  }
  .form-group {
    position: relative;
  }
  input:focus ~ .floating-label,
  input:not(:focus):valid ~ .floating-label,
  textarea:focus ~ .floating-label,
  textarea:not(:focus):valid ~ .floating-label {
    top: -20px;
    right: 20px;
    font-size: 12px;
    opacity: 1;
  }
  .floating-label {
    position: absolute;
    pointer-events: none;
    right: 20px;
    top: 14px;
    transition: 0.2s ease all;
    color: #9ca3af;
    font-size: 14px;
  }
</style>

<script>
  import Counter from '~/components/Counter';
  import { mapGetters } from 'vuex';

  export default {
    components: {
      Counter
    },
    data() {
      return {
        url: 'http://localhost:8000',
        complainModal: false,
        insuranceModal: false,
        prevInsurancesModal: false,
        status: null,
        orderResidences: [],
        insuranceIndex: 0,
        insurances: [],
        prevInsurances: [],
        newInsurance: {
          name: null,
          codeMelli: null,
          birthDate: null
        },
        order: {
          "id": 0,
          "status": 0,
          "amount": 0,
          "off": 0,
          "roffAmount": 0,
          "offId": 0,
          "offAmount": 0,
          "person": 0,
          "extraPerson": 0,
          "extraPersonAmount": 0,
          "priceDefault": 0,
          "daysCount": 0,
          "startDate": "",
          "endDate": "",
          "residence": {
            "id": 0,
            "user": "",
            "title": "",
            "slug": "",
            "province": "",
            "provinceId": 0,
            "city": "",
            "cityId": 0,
            "score": 0
          },
          "image": "",
          "imageUser": ""
        },
        off: null,
        checkingOffCode: false,
        payingWithWallet: false,
        canceling: false,
        cid: 0,
        inid: 0,
        person: 0,
        rid: 0,
        complainSubject: null,
        complainText: null,
        complaining: false,
        sendingInsurance: false,
        deletingInsurance: false,
        changingStatus: false,
        errors: []
      }
    },
    async asyncData({ $axios }) {
      const { data } = await $axios.post(`api/v1/user/orders`,{status:null});
      return {
        orderResidences: data.orderResidences,
        prevInsurances: data.insurances
      };
    },
    methods: {
      setOrder (order) {
        this.order = order;
        this.$store.dispatch('setVuexChackOutMenuState',true);
      },
      closeOrder() {
        this.$store.dispatch('setVuexChackOutMenuState',false);
      },
      async applyOffCode() {
        this.checkingOffCode = true;
        this.errors = [];
        let checkOffCode = await this.$axios.post('api/v1/user/checkOffCode',{id:this.order.id,code:this.off});
        if(checkOffCode.data.status === 1) {
          this.$snotify.success(checkOffCode.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.code = null;
          this.order = checkOffCode.data.order;
        } else if(checkOffCode.data.status === 422) {
          this.errors = checkOffCode.data.errors;
          this.$snotify.warning(checkOffCode.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(checkOffCode.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.checkingOffCode = false;
      },
      async payWithWallet(id) {
        if(!confirm("از پرداخت کردن سفارش شماره "+id+" از طریق کیف پول مطمئن هستید ؟ "))
          return;
        this.payingWithWallet = true;
        this.errors = [];
        let payWithWallet = await this.$axios.post('api/v1/user/payWithWallet',{id:id});
        if(payWithWallet.data.status === 1) {
          this.$snotify.success(payWithWallet.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          let orders = await this.$axios.post(`api/v1/user/orders`,{status:this.status});
          this.orderResidences = orders.data.orderResidences;
          this.$store.dispatch('setVuexChackOutMenuState',false);
        } else if(payWithWallet.data.status === 422) {
          this.errors = payWithWallet.data.errors;
          this.$snotify.warning(payWithWallet.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(payWithWallet.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.payingWithWallet = false;
      },
      async cancelOrder(id) {
        if(!confirm("از کنسل کردن سفارش شماره "+id+" مطمئن هستید ؟ "))
          return;
        this.canceling = true;
        this.errors = [];
        let cancelOrder = await this.$axios.post('api/v1/user/cancelOrder',{id:id});
        if(cancelOrder.data.status === 1) {
          this.$snotify.success(cancelOrder.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          let orders = await this.$axios.post(`api/v1/user/orders`,{status:this.status});
          this.orderResidences = orders.data.orderResidences;
        } else if(cancelOrder.data.status === 422) {
          this.errors = cancelOrder.data.errors;
          this.$snotify.warning(cancelOrder.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(cancelOrder.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.canceling = false;
      },
      async changeStatus(newStauts) {
        this.orderResidences = [];
        this.status = newStauts;
        let orders = await this.$axios.post(`api/v1/user/orders`,{status:this.status});
        this.orderResidences = orders.data.orderResidences;
      },
      startComplain(id) {
        this.cid = id;
        this.complainModal = true;
      },
      async sendComplain() {
        this.complaining = true;
        this.errors = [];
        let sendComplain = await this.$axios.post('api/v1/user/sendComplain',{id:this.cid,complainText:this.complainText,complainSubject:this.complainSubject});
        if(sendComplain.data.status === 1) {
          this.complainModal = false;
          this.$snotify.success(sendComplain.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          this.status = 3;
          let orders = await this.$axios.post(`api/v1/user/orders`,{status:this.status});
          this.orderResidences = orders.data.orderResidences;
        } else if(sendComplain.data.status === 422) {
          this.errors = sendComplain.data.errors;
          this.$snotify.warning(sendComplain.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(sendComplain.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.complaining = false;
      },
      showInsuranceModal(insurances,id,person,rid) {
        this.insurances = insurances;
        this.inid = id;
        this.person = person;
        this.rid = rid;
        this.insuranceModal = true;
      },
      showPrevInsurancesModal() {
        if(this.prevInsurances.length > 0)
          this.prevInsurancesModal = true;
        else
          this.$snotify.error("شما قبلا مسافری همراه خود نداشته اید", {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
      },
      chooseInsurance(prevInsurance) {
        this.newInsurance.name = prevInsurance.name;
        this.newInsurance.codeMelli = prevInsurance.codeMelli;
        this.newInsurance.birthDate = prevInsurance.birthDate;
        this.prevInsurancesModal = false;
        this.insuranceModal = true;
      },
      addInsurance() {
        if(this.insurances.length >= this.person) {
          this.$snotify.warning("بیمه افراد بیشتر مجاز نیست", {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          return;
        }
        this.insurances.push({name:this.newInsurance.name,code_melli:this.newInsurance.codeMelli,birth_date:this.newInsurance.birthDate});
      },
      async sendInsurance() {
        this.sendingInsurance = true;
        this.errors = [];
        let sendInsurance = await this.$axios.post('api/v1/user/sendInsurance',{id:this.inid,insurances:this.insurances});
        if(sendInsurance.data.status === 1) {
          this.newInsurance = {
            name: null,
            codeMelli: null,
            birthDate: null
          };
          this.$snotify.success(sendInsurance.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          let orders = await this.$axios.post(`api/v1/user/orders`,{status:this.status});
          this.orderResidences = orders.data.orderResidences;
          this.prevInsurances = orders.data.insurances;
        } else if(sendInsurance.data.status === 422) {
          this.$snotify.warning(sendInsurance.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(sendInsurance.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.sendingInsurance = false;
      },
      async deleteInsurance(index) {
        this.insurances.splice(index, 1);
      },
      async changeOrderStatus(id,newStatus) {
        this.changingStatus = true;
        this.errors = [];
        let changeOrderStatus = await this.$axios.post('api/v1/user/changeOrderStatus',{id:id,'newStatus':newStatus});
        if(changeOrderStatus.data.status === 1) {
          this.$snotify.success(changeOrderStatus.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
          let orders = await this.$axios.post(`api/v1/user/orders`,{status:this.status});
          this.orderResidences = orders.data.orderResidences;
        } else if(changeOrderStatus.data.status === 422) {
          this.$snotify.warning(changeOrderStatus.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        } else {
          this.$snotify.error(changeOrderStatus.data.msg, {timeout: 2000, showProgressBar: false, closeOnClick: true, pauseOnHover: true});
        }
        this.changingStatus = false;
      },
      number_3_3(num, sep) {
        const number = typeof num === 'number' ? num.toString() : num;
        const separator = typeof sep === 'undefined' ? ',' : sep;
        if(typeof number !== 'undefined')
          return number.replace(/(\d)(?=(\d\d\d)+(?!\d))/g, '$1' + separator);
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
        title: 'سفرهای من - '+this.settings.sitename,
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
