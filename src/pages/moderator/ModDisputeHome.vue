
<template>
  <MainHeaderTop />
  <MainHeaderMid />
  <MainHeaderBottom />
<div class="wrapper">
  <div class="max-w-4xl mx-auto  px-10">
    <div class="grid grid-cols-1 w-full gap-4">

      <nav class="rounded-md">
        <ol class="list-reset flex">
          <li>
            <router-link :to="{ name: 'home' }">
              <a class="text-blue-600 hover:text-blue-700">Home</a>
            </router-link>
          </li>
          <li>
            <span class="text-white mx-2">/</span>
          </li>
          <li>
            <router-link :to="{ name: 'ModHome' }">
              <a class="text-blue-600 hover:text-blue-700">Mod Home</a>
            </router-link>
          </li>
          <li>
            <span class="text-white mx-2">/</span>
          </li>
        </ol>
      </nav>

    </div>
    <div class="text-center text-[20px] mb-5">Mod Disputes Home</div>
    <div class="grid grid-cols-12 gap-5">
      <div class="col-span-12 sm:col-span-4 ">
        <div class="bg-neutral rounded-md p-3">
          <div class="text-[20px] font-bold ">Admin Navigation</div>
          <router-link :to="{ name: 'ModHome' }">
            <div class="hover:underline text-blue-600 hover:text-blue-700 my-5">Mod Home</div>
          </router-link>
          <router-link :to="{ name: 'ModTicketsHome' }">
            <div class="hover:underline text-blue-600 hover:text-blue-700 my-5">Tickets</div>
          </router-link>
          <router-link :to="{ name: 'ModDisputeHome' }">
            <div class="hover:underline text-blue-600 hover:text-blue-700 my-5">Disputes</div>
          </router-link>
        </div>
      </div>


      <div class="col-span-12 sm:col-span-8 mb-20 ">
        <div class="grid grid-cols-12 bg-neutral p-5 rounded-md">

          <div class="col-span-12 mt-5 text-[18px] text-center">Disputes Need Mod Support</div>
          <div class="col-span-12 mt-5">
            <div v-if="needmodorders.length > 0">
              <div v-for="dispute in needmodorders">
                <div class="grid grid-cols-12 border border-1 p-2 rounded bg-gray-400">
                  <div class="col-span-12">Order id: {{ dispute.uuid }}</div>
                  <div class="col-span-6">Vendor: {{ dispute.vendor_user_name }}</div>
                  <div class="col-span-6">
                    Customer: {{ dispute.customer_user_name }}
                  </div>
                  <div class="col-span-12">
                    <div class="my-2 flex">
                      <router-link :to="{
                        name: 'ModDispute',
                        params: { uuid: dispute.uuid },
                      }">
                        <button
                          class="bg-zinc-600 hover:bg-zinc-400 text-white font-bold py-1 px-3 ml-5 mr-5 rounded focus:outline-none focus:shadow-outline"
                          type="button">
                          View Dispute
                        </button>
                      </router-link>
                      <div v-if="dispute.moderator_uuid == null">
                        <button
                          class="bg-yellow-400 hover:bg-yellow-600 text-white font-bold py-1 px-3 ml-5 mr-5 rounded focus:outline-none focus:shadow-outline"
                          type="button" @click="becomemod(dispute.uuid)">
                          Become Moderator
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div v-else class="text-[14px]">
              No disputes need moderators at this time ..
            </div>

            <div class="col-span-12 mt-5 text-[16px] text-center">Disputes being moderatored</div>
            <div class="col-span-12 mt-5">
              <div v-for="dispute in hadmodorders">
                <div class="grid grid-cols-12 border border-1 p-2 rounded bg-gray-400">
                  <div class="col-span-12">Order id: {{ dispute.uuid }}</div>
                  <div class="col-span-6">Vendor: {{ dispute.vendor_user_name }}</div>
                  <div class="col-span-6">
                    Customer: {{ dispute.customer_user_name }}
                  </div>
                  <div class="col-span-12">
                    Modded by: {{ dispute.moderator_uuid }}
                  </div>
                  <div class="col-span-12">
                    <div class="my-2 flex">
                      <router-link :to="{
                        name: 'ModDispute',
                        params: { uuid: dispute.uuid },
                      }">
                        <button
                          class="bg-zinc-600 hover:bg-zinc-400 text-white font-bold py-1 px-3 ml-5 mr-5 rounded focus:outline-none focus:shadow-outline"
                          type="button">
                          View Dispute
                        </button>
                      </router-link>
                    </div>
                  </div>
                </div>
              </div>
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
import authHeader from "../../services/auth.header";
import MainHeaderTop from "../../layouts/headers/MainHeaderTop.vue";
import MainHeaderMid from "../../layouts/headers/MainHeaderMid.vue";
import MainHeaderBottom from "../../layouts/headers/MainHeaderBottom.vue";
import MainHeaderVendor from "../../layouts/headers/MainHeaderVendor.vue";
import MainFooter from "../../layouts/footers/FooterMain.vue";
import { formatDistance } from "date-fns";
export default defineComponent({
  name: "ModDisputesHome",
  components: {
    MainHeaderTop,
    MainHeaderMid,
    MainHeaderBottom,
    MainHeaderVendor,
    MainFooter,
  },
  data () {
    return {
      interval: null,
      needmodorders: [],
      hadmodorders: [],
    };
  },
  mounted () {
    this.userstatus();
    this.getdisputesneedmod();
    this.getdisputeshasmod();
  },
  created () {
    this.interval = setInterval(() => {
      this.getdisputesneedmod();
      this.getdisputeshasmod();
    }, 30000);
  },
  destroyed () {
    clearInterval(this.interval)
  },
  methods: {
    // get user status
    userstatus () {
      axios({
        method: "get",
        url: "/auth/whoami",
        withCredentials: true,
        headers: authHeader(),
      })
        .then((response) => {
         if (response.data.login == true) {
            if (response.data.user.user_admin < 2) {
              this.$router.push({ name: "home" });
            }
          }
        });
    },
    // date conversion
    relativeDate (value: any) {
      let e = new Date(value).valueOf();
      return formatDistance(e, new Date());
    },
    // become the moderator of the order
    becomemod (uuid: string) {
      axios({
        method: "put",
        url: "/mod/takeonmod/" + uuid ,
        withCredentials: true,
        headers: authHeader(),
      })
        .then((response) => {
          if (response.data.success) {
            this.getdisputesneedmod();
            this.getdisputeshasmod();
          }
        });
    },
    // get the disputes that need mods
    getdisputesneedmod () {
      axios({
        method: "get",
        url: "/mod/disputes/available",
        withCredentials: true,
        headers: authHeader(),
      })
        .then((response) => {
          if (response.data.success) {
            this.needmodorders = response.data;
           
          }
        });
    },
    // get disputes that already has mods
    getdisputeshasmod () {
      axios({
        method: "get",
        url: "/mod/disputes/modded",
        withCredentials: true,
        headers: authHeader(),
      })
        .then((response) => {
          if (response.data.success) {
            this.hadmodorders = response.data;
          }
        });
    },
  },
});
</script>
