
<template>
  <div v-if="loaded === true">
    <MainHeaderTop />
    <MainHeaderMid />
    <MainHeaderBottom />
    <div v-if="user">
      <MainHeaderVendor v-show="user.user_admin === 1" />
    </div>

    <div class="max-w-7xl mx-auto wrapper">

      <div class="grid grid-cols-1 w-full gap-4">
        <div class="mb-5 mt-5 px-5">
          <nav class="rounded-md">
            <ol class="list-reset flex">
              <li>
                <router-link :to="{ name: 'home' }">
                  <a class="text-blue-600 hover:text-blue-700">Home</a>
                </router-link>
              </li>
              <li>
                <span class="text-gray-500 mx-2">/</span>
              </li>
              <li>
                <router-link :to="{ name: 'cart' }">
                  <a class="text-blue-600 hover:text-blue-700">Shopping Cart</a>
                </router-link>
              </li>
              <li>
                <span class="text-gray-500 mx-2">/</span>
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <div v-if="shopping_cart_items_list_count == 1">
        <div class="text-center text-[28px] mb-10">Checkout ({{ shopping_cart_items_list_count }} item)</div>
      </div>
      <div v-else>
        <div class="text-center text-[28px] mb-10">Checkout ({{ shopping_cart_items_list_count }} items)</div>
      </div>
      <div class="grid grid-cols-12 w-full gap-4 px-2">
        <div class="col-span-12 md:col-span-8 grid-rows-3 bg-neutral rounded-md p-5 mb-5 ">
          <div class="grid grid-cols-12 border-b border-gray-300 pb-4 ">
            <div class="col-span-1 font-bold">1</div>
            <div class="col-span-3 font-bold text-[20px]">Shipping Options</div>
            <div class="col-span-6" v-if="address_name.length > 5">
              <div class="grid grid-cols-1">
                <div class="">{{ address_name }}</div>
                <div class="">{{ address }}</div>
                <div class="">{{ apt }}</div>
                <div class="flex">
                  <div class="pr-1">{{ city }}</div>
                  <div class="px-1">{{ stateorprovence }}</div>
                  <div class="px-1">{{ zip }}</div>
                </div>
              </div>
            </div>
            <div v-else class="col-span-6 font-bold text-red-600">
              Please Enter a shipping address to Checkout
            </div>
            <div class="col-span-2 text-center text-[12px] text-blue-600 hover:text-blue-400">
              <router-link :to="{ name: 'defaultaddress' }"> Change/Add </router-link>
            </div>
          </div>
          <div class="grid grid-cols-12 pb-4 pt-4 border-b border-gray-300 mb-10">
            <div class="col-span-1 font-bold">2</div>
            <div class="col-span-3 font-bold text-[20px]">Wallet Balances</div>
            <div class="col-span-6">
              <div class="grid grid-cols-1">
                <div class="text-[14px]">
                  <div v-if="btcbalance == 0">BTC Balance: 0.00000000</div>
                  <div v-else>BTC Balance: {{ btcbalance }}</div>
                </div>
                <div class="text-[14px]">
                  <div v-if="bchbalance == 0">BCH Balance: 0.00000000</div>
                  <div v-else>BCH Balance: {{ bchbalance }}</div>
                </div>
                <div class="text-[14px]">
                  <div v-if="xmrbalance == 0">XMR Balance: 0.000000000000</div>
                  <div v-else>XMR Balance: {{ xmrbalance }}</div>
                </div>
              </div>
            </div>

            <div class="col-span-2 text-center text-[12px]"></div>
          </div>
          <div class="grid grid-cols-12 pb-4">
            <div class="col-span-1 font-bold">3</div>
            <div class="col-span-6 font-bold text-[20px]">Review Items and Shipping</div>
            <div class="col-span-11 mt-5">
              <div v-for="item in shopping_cart_items_list">
                <div class="grid grid-cols-12 gap-4 border-t border-gray-300 pt-5 ">
                  <div class="col-span-12 flex justify-center md:col-span-2">
                    <img class="h-24" :src="item.image_of_item" alt="" />
                  </div>
                  <div class="col-span-12 md:col-span-6 text-center">
                    <div class="grid grid-cols-1">
                      <div class="col-span-1 font-bold pb-4">
                        {{ item.title_of_item }}
                      </div>
                      <div class="col-span-1 flex gap-5">
                        <div class="font-bold">Item Price:</div>
                        <div class="">{{ item.price_of_item }}$</div>
                      </div>
                      <div class="col-span-1 flex gap-5">
                        <div class="font-bold"> Item Count: </div>
                        <div class="">{{ item.quantity_of_item }} items</div>
                      </div>
                      <div class="col-span-1 flex gap-5">
                        <div class="font-bold">
                          Shipping Selected:
                        </div>
                        <div class="">{{ item.selected_shipping_description }}</div>
                      </div>
                    </div>
                  </div>
                  <div class="col-span-12 md:col-span-4 ">

                    <div v-if="item.digital_currency_1 === true">
                      <div class=" flex gap-5 mb-2">
                        <input v-on:change="checkoutpaymenttype($event, item)" v-model="item.selected_currency"
                          type="radio" :id="item.id" :name="item.id" value="1"
                          :checked="item.selected_digital_currency === 1" />
                        <label class="px-5" for="btc">Bitcoin</label><br />
                      </div>
                    </div>
                    <div v-if="item.digital_currency_2 === true">
                      <div class=" flex gap-5 mb-2">
                        <input v-on:change="checkoutpaymenttype($event, item)" v-model="item.selected_currency"
                          type="radio" :id="item.id" :name="item.id" value="2"
                          :checked="item.selected_digital_currency === 2" />
                        <label class="px-5 " for="bch">Bitcoin Cash</label><br />
                      </div>
                    </div>
                    <div v-if="item.digital_currency_3 === true">
                      <div class=" flex gap-5 mb-2">
                        <input v-on:change="checkoutpaymenttype($event, item)" v-model="item.selected_currency"
                          type="radio" :id="item.id" :name="item.id" value="3"
                          :checked="item.selected_digital_currency === 3" />
                        <label class="px-5" for="xmr">Monero</label>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-span-12 md:col-span-4 px-10 mb-5">
          <div class="grid grid-cols-1 bg-neutral rounded-md p-5">
            <div class="col-span-1">
              <h1 class="font-semibold text-2xl border-b pb-8">Order Summary</h1>
            </div>
            <div class="mb-5">
              <div v-if="btctotalprice > 0">
                <div class="col-span-1">
                  <span class="font-semibold text-sm uppercase">Bitcoin </span>
                  <div class="text-[14px]">Items: {{ btcsumofitem }}</div>
                  <div class="text-[14px]">Price: {{ btcprice }}</div>
                  <div class="text-[14px]">
                    Shipping Price: {{ btcshippingprice }}
                  </div>
                  <div class="text-[14px]">Total Price: {{ btctotalprice }}</div>
                </div>
              </div>
              <div v-if="bchtotalprice > 0">
                <div class="col-span-1">
                  <span class="font-semibold text-sm uppercase">Bitcoin Cash</span>
                  <div class="text-[14px]">Items: {{ bchsumofitem }}</div>
                  <div class="text-[14px]">Price: {{ bchprice }}</div>
                  <div class="text-[14px]">
                    Shipping Price: {{ bchshippingprice }}
                  </div>
                  <div class="text-[14px]">Total Price: {{ bchtotalprice }}</div>
                </div>
              </div>
              <div v-if="xmrtotalprice > 0">
                <div class="col-span-1">
                  <span class="font-semibold text-sm uppercase ">Monero</span>
                  <div class="text-[14px]">Items: {{ xmrsumofitem }}</div>
                  <div class="text-[14px]">Price: {{ xmrprice }}</div>
                  <div class="text-[14px]">
                    Shipping Price: {{ xmrshippingprice }}
                  </div>
                  <div class="text-[14px]">Total Price: {{ xmrtotalprice }}</div>
                </div>
              </div>
              <div class="mt-5 mb-5">
                <div v-if="xmrtotalprice <= xmrbalance &&
                  bchtotalprice <= bchbalance &&
                  btctotalprice <= btcbalance">
                </div>
                <div v-else class="text-red-600 text-center font-bold">Not Enough Coin in your wallet</div>
                <div v-if="address_name.length > 5">
                </div>
                <div v-else class="text-red-600 text-center font-bold">Need A Shipping Address</div>
                <button v-show="
                  xmrtotalprice <= xmrbalance &&
                  bchtotalprice <= bchbalance &&
                  btctotalprice <= btcbalance &&
                  address_name.length > 5
                " @click="checkoutorder()"
                  class="bg-yellow-500 bg-r rounded-md font-semibold hover:bg-yellow-600 py-3 text-sm text-white uppercase w-full">
                  Place Order
                </button>
                <span class="flex font-semibold text-[10px] uppercase text-center mt-10">By Clicking checkout, you agree to the terms
                  of Freeport </span>
                <router-link :to="{ name: 'policies' }">
                  <div class="text-blue-600 hover:text-blue-500 text-sm font-bold flex justify-center">
                    Freeport Terms
                  </div>
                </router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <MainFooter />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import { notify } from "@kyvg/vue3-notification";
import authHeader from "../../services/auth.header";
import MainHeaderTop from "../../layouts/headers/MainHeaderTop.vue";
import MainHeaderMid from "../../layouts/headers/MainHeaderMid.vue";
import MainHeaderBottom from "../../layouts/headers/MainHeaderBottom.vue";
import MainHeaderVendor from "../../layouts/headers/MainHeaderVendor.vue";
import MainFooter from "../../layouts/footers/FooterMain.vue";


export default defineComponent({
  name: "Checkout",

  components: {
    MainHeaderTop,
    MainHeaderMid,
    MainHeaderBottom,
    MainHeaderVendor,
    MainFooter,
  },


  data () {
    return {
      loaded: false,
      user: null,
      shopping_cart_items_list: [],
      shopping_cart_items_list_count: 0,
      xmrbalance: 0,
      bchbalance: 0,
      btcbalance: 0,
      btcsumofitem: 0,
      cart_status: null,
      selectedpayment: null,
      btcprice: "",
      btcshippingprice: "",
      btctotalprice: 0,
      bchsumofitem: "",
      bchprice: "",
      bchshippingprice: "",
      bchtotalprice: 0,
      xmrsumofitem: "",
      xmrprice: "",
      xmrshippingprice: "",
      xmrtotalprice: 0,
      address_name: "",
      countryList: "",
      country: "",
      address: "",
      apt: "",
      city: "",
      stateorprovence: "",
      zip: "",
      message: "",
      interval: null,
    };
  },

 
  created () {
    this.userstatus();
    this.interval = setInterval(() => {
      this.updateprices();
    }, 50000);
  },
   mounted () {
    this.getxmrprice();
    this.getbchprice();
    this.getbtcprice();
    this.get_shopping_cart_items();
    this.get_shopping_cart_items_count();
    this.updateprices();
    this.get_shopping_cart_order_summary();
    this.getcurrentshipping();
  },
  methods: {
    userstatus () {
      axios({
        method: "get",
        url: "/auth/whoami",
        withCredentials: true,
        headers: authHeader(),
      })
        .then((response) => {
          if (response.data.login == true) {
            this.user = response.data.user;
          }
        })
        .catch(() => {
          this.$router.push({ name: "login" });
        });
    },

    checkoutorder () {
      axios({
        method: "post",
        url: "/checkout/payment",
        withCredentials: true,
        headers: authHeader(),
      })
        .then((response) => {
          if (response.data.success) {

            notify({
              title: "Freeport Message",
              text: "Successfully completed order!",
              type: "success",
            });
            this.$router.push({ name: "userorders" });
          }
          if (response.data.error) {
            notify({
              title: "Error Checking Out",
              text: response.data.error + "poop",
              type: "error",
            });
          }
        })
        .catch((error) => {
          notify({
            title: "Error Checking Out",
            text: error,
            type: "error",
          });
        });
    },
    updateprices () {
      axios({
        method: "get",
        url: "/checkout/update/price",
        withCredentials: true,
        headers: authHeader(),
      })
        .then((response) => {
          if (response.data.success) { }
        })
    },
    get_shopping_cart_items () {
      axios({
        method: "get",
        url: "/checkout/data/incart",
        headers: authHeader(),
        withCredentials: true,
      })
        .then((response) => {

          if (response.data.error) {
            this.shopping_cart_items_list = null;
          }
          else {
            this.shopping_cart_items_list = response.data;
          }
        })
        .catch(() => {
          this.shopping_cart_items_list = null;
        });
    },
    get_shopping_cart_items_count () {
      axios({
        method: "get",
        url: "/checkout/data/incart/count",
        headers: authHeader(),
        withCredentials: true,
      })
        .then((response) => {

          if (response.data.success) {
            this.shopping_cart_items_list_count = response.data.cart_count;
          }
          else {
            this.shopping_cart_items_list_count = null;
          }
        })
        .catch(() => {
          this.shopping_cart_items_list_count = null;
        });
    },
    checkoutpaymenttype (event: any, item: any) {
      this.selectedpayment = event.target.value;
      let payLoad = {
        new_currency: this.selectedpayment,
      };
      axios({
        method: "post",
        url: "/checkout/changepaymentoption/" + item.id,
        headers: authHeader(),
        data: payLoad,
      })
        .then(() => {
          this.get_shopping_cart_order_summary();
        })
        .catch(() => {
          this.shopping_cart_items_list = null;
        });
    },

    get_shopping_cart_order_summary () {
      axios({
        method: "get",
        url: "/checkout/data/cart/total",
        headers: authHeader(),
      })
        .then((response) => {

          if (response.data.success) {
            this.btcsumofitem = response.data.btc_sum_of_item;
            this.btcprice = response.data.btc_price;
            this.btcshippingprice = response.data.btc_shipping_price;
            this.btctotalprice = response.data.btc_total_price;
            this.bchsumofitem = response.data.bch_sum_of_item;
            this.bchprice = response.data.bch_price;
            this.bchshippingprice = response.data.bch_shipping_price;
            this.bchtotalprice = response.data.bch_total_price;
            this.xmrsumofitem = response.data.xmr_sum_of_item;
            this.xmrprice = response.data.xmr_price;
            this.xmrshippingprice = response.data.xmr_shipping_price;
            this.xmrtotalprice = response.data.xmr_total_price;
          }
        }).catch(() => { });
    },
    getcurrentshipping () {
      axios({
        method: "get",
        url: "/info/getdefaultaddress",
        withCredentials: true,
        headers: authHeader(),
      })
        .then((response) => {
          if (response.data.success) {
            this.address_name = response.data.address_name;
            this.country = response.data.country;
            this.address = response.data.address;
            this.apt = response.data.apt;
            this.city = response.data.city;
            this.address = response.data.address;
            this.stateorprovence = response.data.state_or_provence;
            this.zip = response.data.zip;
            this.message = response.data.message;
            this.loaded = true;
          }
        });
    },
    //  Get prices of current coins
    getxmrprice () {
      axios({
        method: "get",
        url: "/xmr/balance",
        headers: authHeader(),
      })
        .then((response) => {
          if (response.data.success) {
            this.xmrbalance = response.data.xmr_balance;
          }
        })
        .catch(() => {
          this.shopping_cart_items_list = null;
        });
    },
    getbchprice () {
      axios({
        method: "get",
        url: "/bch/balance",
        headers: authHeader(),
      })
        .then((response) => {
          if (response.data.success) {
            this.bchbalance = response.data.bch_balance;
          }
        })
        .catch(() => {
          this.shopping_cart_items_list = null;
        });
    },
    getbtcprice () {
      axios({
        method: "get",
        url: "/btc/balance",
        headers: authHeader(),
      })
        .then((response) => {
          if (response.data.success) {
            this.btcbalance = response.data.btc_balance;
          }
        })
        .catch(() => {
          this.shopping_cart_items_list = null;

        });
    },
  },
});
</script>
