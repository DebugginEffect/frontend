<template>
    <MainHeaderTop />
    <MainHeaderMid />
    <MainHeaderBottom />

    <div v-if="user">
        <MainHeaderVendor v-show="user.user_admin === 1" />
    </div>

    <div class="container max-w-7xl mx-auto px-10 mt-2 pb-72 text-white">
        <div class="mt-5 mb-5">
            <nav class="rounded-md w-full">
                <ol class="list-reset flex">
                    <li>
                        <router-link :to="{ name: 'home' }">
                            <a class="text-blue-600 hover:text-blue-700">Home</a>
                        </router-link>
                    </li>
                    <li>
                        <span class="text-white mx-2 mb-2 mt-2 bg-">/</span>
                    </li>
                </ol>
            </nav>
        </div>
        <div class="bg-neutral rounded-md p-5">
            <div class="col-12 text-dark border-bottom mb-3">
                <h3>Crypto Disclaimer</h3>
                <br />
                <h5>Overview</h5>
                <br />
                Cryptocurrency is an Open Source Software. It is not guaranteed by any formal government backing entity
                and as such should not be considered to have Warranties of any kind, implied or elsewise. Be careful
                with digital irreversible currency, particularly with software downloaded and strangers soliciting you.
                Use digital common sense and keep a high level of skepticism with online communications from anyone. We
                do are best to enforce security, but
            </div>
            <div class="col-12 text-dark border-bottom mb-2">
                <h5>Risks Related to cryptocurrency</h5>

                The Website will not be responsible for any losses, damages or claims arising from events falling within
                the scope of the following five categories: (1) Mistakes made by the user of any Bitcoin-related
                software or service, e.g., forgotten passwords, payments sent to wrong Bitcoin addresses, and accidental
                deletion of wallets. (2) Software problems of the Website and/or any Bitcoin-related software or
                service, e.g., corrupted wallet file, incorrectly constructed transactions, unsafe cryptographic
                libraries, malware affecting the Website and/or any Bitcoin-related software or service. (3) Technical
                failures in the hardware of the user of any Bitcoin-related software or service, e.g., data loss due to
                a faulty or damaged storage device. (4) Security problems experienced by the user of any Bitcoin-related
                software or service, e.g., unauthorized access to users' wallets and/or accounts. (5) Actions or
                inactions of third parties and/or events experienced by third parties, e.g., bankruptcy of service
                providers, information security attacks on service providers, and fraud conducted by third parties.
            </div>
            <div class="col-12 text-dark border-bottom mb-2">
                <h5>Compliance with taxes</h5>

                We are not responsible for tax information, collection of information related to taxes, or reporting
                taxes for you. You are responsible for taxes in your country and reporting of all transactions.
            </div>
            <div class="col-12 text-dark border-bottom mb-2">
                <h5>Warrenty website</h5>
                This website is provided " as is" and we are not responsible for any content, data, materials, or
                services provided by the website.
            </div>
            <div class="col-12 text-dark border-bottom mb-3 mt-5">
                <h3>Disclaimer</h3>
                Last updated December 26, 2022
            </div>

            <div class="col-12 text-dark border-bottom mb-2">
                <h5>WEBSITE DISCLAIMER</h5>
                The information provided by freeport.cash (“we,” “us” or “our”) on http://www.freeport.cash (the “Site”)
                and our mobile application is for general informational purposes only. All information on the Site and
                our mobile application is provided in good faith, however we make no representation or warranty of any
                kind, express or implied, regarding the accuracy, adequacy, validity, reliability, availability or
                completeness of any information on the Site or our mobile application. UNDER NO CIRCUMSTANCE SHALL WE
                HAVE ANY LIABILITY TO YOU FOR ANY LOSS OR DAMAGE OF ANY KIND INCURRED AS A RESULT OF THE USE OF THE SITE
                OR OUR MOBILE APPLICATION OR RELIANCE ON ANY INFORMATION PROVIDED ON THE SITE AND OUR MOBILE
                APPLICATION. YOUR USE OF THE SITE AND OUR MOBILE APPLICATION AND YOUR RELIANCE ON ANY INFORMATION ON THE
                SITE AND OUR MOBILE APPLICATION IS SOLELY AT YOUR OWN RISK.
            </div>
            <div class="col-12 text-dark border-bottom mb-2">
                <h5>EXTERNAL LINKS DISCLAIMER</h5>
                The Site and our mobile application may contain (or you may be sent through the Site or our mobile
                application) links to other websites or content belonging to or originating from third parties or links
                to websites and features in banners or other advertising. Such external links are not investigated,
                monitored, or checked for accuracy, adequacy, validity, reliability, availability or completeness by us.
                WE DO NOT WARRANT, ENDORSE, GUARANTEE, OR ASSUME RESPONSIBILITY FOR THE ACCURACY OR RELIABILITY OF ANY
                INFORMATION OFFERED BY THIRD-PARTY WEBSITES LINKED THROUGH THE SITE OR ANY WEBSITE OR FEATURE LINKED IN
                ANY BANNER OR OTHER ADVERTISING. WE WILL NOT BE A PARTY TO OR IN ANY WAY BE RESPONSIBLE FOR MONITORING
                ANY TRANSACTION BETWEEN YOU AND THIRD-PARTY PROVIDERS OF PRODUCTS OR SERVICES.
            </div>
            <div class="col-12 text-dark border-bottom mb-2">
                <h5>TESTIMONIALS DISCLAIMER</h5>

                The Site may contain testimonials by users of our products and/or services. These testimonials reflect
                the real-life experiences and opinions of such users. However, the experiences are personal to those
                particular users, and may not necessarily be representative of all users of our products and/or
                services. We do not claim, and you should not assume, that all users will have the same experiences.
                YOUR INDIVIDUAL RESULTS MAY VARY. The testimonials on the Site are submitted in various forms such as
                text, audio and/or video, and are reviewed by us before being posted. They appear on the Site verbatim
                as given by the users, except for the correction of grammar or typing errors. Some testimonials may have
                been shortened for the sake of brevity where the full testimonial contained extraneous information not
                relevant to the general public. The views and opinions contained in the testimonials belong solely to
                the individual user and do not reflect our views and opinions. Freeport.cash
            </div>
        </div>
    </div>

    <MainFooter />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import MainHeaderTop from '../../layouts/headers/MainHeaderTop.vue'
import MainHeaderMid from '../../layouts/headers/MainHeaderMid.vue'
import MainHeaderBottom from '../../layouts/headers/MainHeaderBottom.vue'
import MainHeaderVendor from '../../layouts/headers/MainHeaderVendor.vue'
import MainFooter from '../../layouts/footers/FooterMain.vue'

export default defineComponent({
    name: 'Legal',
    components: {
        MainHeaderTop,
        MainHeaderMid,
        MainHeaderBottom,
        MainHeaderVendor,
        MainFooter,
    },
    data() {
        return {
            user: null,
        }
    },

    mounted() {},

    methods: {},
})
</script>
