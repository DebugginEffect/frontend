
<template>
    <MainHeaderTop />
    <MainHeaderMid />
    <MainHeaderBottom />
    <div class="wrapper">

        <div class="container max-w-5xl mx-auto px-10  mb-10 text-white">
            <div class="grid grid-cols-1 w-full gap-4 mb-5">
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
                            <router-link :to="{ name: 'supporthome' }">
                                <a class="text-blue-600 hover:text-blue-700">Customer Support</a>
                            </router-link>
                        </li>
                        <li>
                            <span class="text-gray-500 mx-2">/</span>
                        </li>
                        <li>
                            <router-link :to="{ name: 'supportticket' }">
                                <a class="text-blue-600 hover:text-blue-700">Ticket Center</a>
                            </router-link>
                        </li>
                        <li>
                            <span class="text-gray-500 mx-2">/</span>
                        </li>
                    </ol>
                </nav>
            </div>

            <div v-if="loaded">
                <div class="grid grid-cols-12 gap-5">
                    <div class="col-span-12 sm:col-span-4 ">
                        <div class="bg-neutral rounded-md p-5">
                            <div class="text-[18px] mb-5">Tickets</div>
                            <div v-if="all_tickets.length > 0">
                                <div v-for="ticket in all_tickets" :key="ticket.id">
                                    <div class="grid grid-cols-12 border-b-2 border-gray-400 mb-5"
                                        v-if="ticket !== null">
                                        <router-link class="col-span-12"
                                            :to="{ name: 'supportviewticket', params: { uuid: ticket.uuid } }">
                                            <div
                                                class="col-span-12 text-blue-700 hover:underline hover:text-blue-500 text-[16px] overflow-hidden">
                                                {{ ticket.subject }}
                                            </div>
                                        </router-link>
                                        <router-link :to="{ name: 'supportviewticket', params: { uuid: ticket.uuid } }">

                                            <div class="col-span-12 "> {{
                                                ticket.uuid
                                            }}</div>
                                        </router-link>
                                        <div class="col-span-12 flex gap-5 mt-2">
                                            <div class="">
                                                Created:
                                            </div>
                                            <div class="">
                                                {{ relativeDate(ticket.timestamp) }} ago
                                            </div>
                                        </div>
                                        <div v-if="ticket.status == 0" class="col-span-12 ">
                                            <div class="flex gap-3 col-span-12">
                                                <div class="">Status:</div>
                                                <div class="text-red-600">Closed</div>
                                            </div>

                                        </div>
                                        <div v-else-if="ticket.status == 1" class="col-span-12">
                                            <div class="flex gap-3 col-span-12">
                                                <div class="">Status:</div>
                                                <div class="text-green-600">Open</div>
                                            </div>

                                        </div>
                                        <div v-else class="col-span-12">
                                            <div class="flex gap-3 col-span-12 ">
                                                <div class="">Status:</div>
                                                <div class=" text-orange-400">New MSG</div>
                                            </div>

                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div v-else>
                                No Previous Tickets
                            </div>
                        </div>
                    </div>
                    <div class="col-span-12 sm:col-span-8">
                        <div v-if="get_ticket !== null">
                            <div class="text-[18px] mb-3 justify-center flex">
                                Ticket # {{ get_ticket.uuid }}:
                            </div>
                            <div class="flex col-span-12 justify-center">
                                <div v-if="get_ticket.status == 0">
                                    <div class="flex gap-3">
                                        <div class="">Status:</div>
                                        <div class="text-red-600">Closed</div>
                                    </div>
                                </div>
                                <div v-else-if="get_ticket.status == 1">
                                    <div class="flex gap-3">
                                        <div class="">Status:</div>
                                        <div class="text-green-600">Open</div>
                                    </div>
                                </div>
                                <div v-else>
                                    <div class="flex gap-3">
                                        <div class="">Status:</div>
                                        <div class=" text-orange-400">New MSG</div>
                                    </div>
                                </div>
                            </div>
                            <form class="rounded-md pt-6 pb-8 mb-4 w-full bg-neutral p-5" @submit.prevent="onSubmit"
                                v-if="get_ticket.status !== 0">
                                <div class="">
                                    <textarea v-model="SendMsgForm.msginfo" id="item_description"
                                        placeholder="Write something .." class="shadow appearance-none border rounded w-full py-2 px-3
                                                        text-white leading-tight
                                                        focus:outline-none focus:shadow-outline mb-3">
                                        </textarea>
                                    <span v-if="v$.SendMsgForm.msginfo.$error" class="text-red-600 text-center">
                                        {{ v$.SendMsgForm.msginfo.$errors[0].$message }}
                                    </span>
                                </div>
                                <div class="flex justify-end">
                                    <button class="bg-gray-600 hover:bg-zinc-400 text-white font-bold 
                                                                    py-2 px-4 rounded
                                                                    focus:outline-none focus:shadow-outline" type="submit">
                                        Send Message
                                    </button>
                                </div>
                            </form>

                            <div class="grid grid-cols-12 p-5  bg-neutral rounded-md">
                                <div v-for="comment in get_ticket_data" :key="comment.id" class="col-span-12">
                                    <div class="grid grid-cols-12 mb-5">
                                        <div v-if="comment.author_uuid == user.user_id"
                                            class="col-span-12 flex justify-start">
                                            <div class="col-span-12 text-white">
                                                <router-link
                                                    class="hover:text-blue-500 hover:underline  font-bold text-[18px]" :to="{
                                                        name: 'userprofile',
                                                        params: { uuid: comment.author_uuid },
                                                    }">
                                                    {{ comment.author }}
                                                </router-link>
                                            </div>
                                            <div class="">
                                                - {{ relativeDate(comment.timestamp) }} ago
                                            </div>

                                        </div>
                                        <div v-else class="col-span-12 flex justify-end">
                                            <div class="col-span-12 text-yellow-600 font-bold text-[18px]">
                                                {{ comment.author }} [ADMIN]
                                            </div>
                                            <div class="">
                                                - {{ relativeDate(comment.timestamp) }} ago
                                            </div>
                                        </div>

                                        <div v-if="comment.author_uuid == user.user_id"
                                            class="col-span-12 flex justify-start">
                                            <div class="col-span-12 text-white bg-blue-500 p-3 border rounded-md">
                                                {{ comment.text_body }}
                                            </div>
                                        </div>
                                        <div v-else class="col-span-12 flex justify-end">
                                            <div class="col-span-12 text-white bg-gray-700 p-3 border rounded-md">
                                                {{ comment.text_body }}
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
import { notify } from "@kyvg/vue3-notification";
import useValidate from "@vuelidate/core";
import { required, minLength } from "@vuelidate/validators";
import authHeader from "../../services/auth.header";
import MainHeaderTop from "../../layouts/headers/MainHeaderTop.vue";
import MainHeaderMid from "../../layouts/headers/MainHeaderMid.vue";
import MainHeaderBottom from "../../layouts/headers/MainHeaderBottom.vue";
import MainHeaderVendor from "../../layouts/headers/MainHeaderVendor.vue";
import MainFooter from "../../layouts/footers/FooterMain.vue";
import { formatDistance } from "date-fns";
export default defineComponent({
    name: "supportviewticket",

    components: {
        MainHeaderTop,
        MainHeaderMid,
        MainHeaderBottom,
        MainHeaderVendor,
        MainFooter,
    },

    data () {
        return {
            v$: useValidate(),
            loaded: false,
            get_ticket: null,
            get_ticket_data: [],
            userlist: [],
            user: null,
            all_tickets: [],
            interval: null,
            SendMsgForm: {
                msginfo: "",
            },
        };
    },
    created(){
        this.userstatus();
    },
    mounted () {
        this.get_all_tickets();
        this.get_current_ticket();
        this.get_current_ticket_messages();
        this.interval = setInterval(() => {
            this.get_current_ticket();
            this.get_current_ticket_messages();
        }, 50000);

    },

    destroyed  () {
        clearInterval(this.interval)
    },
    validations () {
        return {
            SendMsgForm: {
                msginfo: { required, minLength: minLength(4) },
            },
        };
    },
    watch: {
        $route () {
            this.get_ticket == null;
            this.get_ticket_data == null;
            this.get_ticket.uuid = this.$route.params.uuid;
            this.get_current_ticket();
            this.get_current_ticket_messages();
        },
    },

    methods: {
        relativeDate (value: any) {
            let e = new Date(value).valueOf();
            return formatDistance(e, new Date());
        },
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
                    this.user = null;
                });
        },
        markasread () {
            axios({
                method: "put",
                url: "/customer-service/markasread/" + this.get_ticket.uuid,
                withCredentials: true,
                headers: authHeader(),
            })
                .then((response) => {
                    if (response.data.success) {
                        this.get_all_tickets();
                    }
                })
                .catch(() => {
                    this.user = null;
                });
        },

        get_current_ticket () {
            let url = this.$route.params.uuid
            if (url == null){
                clearInterval(this.interval)
                this.interval = null;
            }else{
   
            axios({
                method: "post",
                url: "/customer-service/ticket/" + url,
                withCredentials: true,
              
                headers: authHeader(),
            })
                .then((response) => {
                    if (response.data.success) {
                        this.get_ticket = response.data
                    }
                });
                   }
        },
        get_current_ticket_messages () {
            let url = this.$route.params.uuid
            axios({
                method: "post",
                url: "/customer-service/ticket/messages/" + url,
                withCredentials: true,
            
                headers: authHeader(),
            })
                .then((response) => {
                    if (response.data.success) {
                        this.get_ticket_data = response.data;

                        this.loaded = true;
                        if (this.get_ticket != null) {
                            this.markasread();
                        }

                    }
                });
        },
        get_all_tickets () {
            axios({
                method: "get",
                url: "/customer-service/tickets",
                withCredentials: true,
                headers: authHeader(),
            })
                .then((response) => {
                    if (response.data.success) {
                        this.all_tickets = response.data;
                    }
                });
        },

        sendMessage (payLoad: { textbody: string, ticketid: any }) {
            axios({
                method: "post",
                url: "/customer-service/create/ticket/comment",
                data: payLoad,
                withCredentials: true,
                headers: authHeader(),
            })
                .then((response) => {
                    if (response.data.success) {
                        this.get_current_ticket_messages();
                    }
                })
                .catch((error) => {

                    notify({
                        title: "Freeport Error",
                        text: "Error posting information.",
                        type: "error",
                    });
                });
        },
        onSubmit () {
            let payLoad = { textbody: this.SendMsgForm.msginfo, ticketid: this.$route.params.uuid };
            this.v$.$validate(); // checks all inputs
            if (this.v$.$invalid) {
                notify({
                    title: "Message",
                    text: "Form Failure",
                    type: "error",
                });
            this.SendMsgForm.msginfo = '';
            }
            else {


                this.sendMessage(payLoad);
            }
        },
    },
});
</script>

