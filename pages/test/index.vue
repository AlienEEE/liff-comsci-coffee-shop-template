<template>
    <div style="overflow: hidden;">
        <v-app-bar  dense flat >
            <v-toolbar-title class="pa-0 mb-3">Test</v-toolbar-title>
        </v-app-bar>
        <v-main >


            <v-container  class="pt-10 pb-0 pr-0 pl-0"  >
                <v-row>
                    <v-col  >
                 
                        <div class="text-subtitle text-center">Get UserID by LineTokenID 1</div>
                    
                    </v-col>
                   
                </v-row>
                <v-row>
                   
                    <v-col  >
                 
                        <div class="text-subtitle text-center  red">{{userId1}}</div>
         
                    </v-col>
                </v-row>
                <v-row>
                    <v-col  >
                 
                        <div class="text-subtitle text-center">Get UserID by LineTokenID 2</div>
                    
                    </v-col>
                   
                </v-row>
                <v-row>
                   
                    <v-col  >
                 
                        <div class="text-subtitle text-center  red">{{userId2}}</div>
         
                    </v-col>
                </v-row>
                <v-row>
                    <v-col  class="pl-10 pr-10">
                 
                        <v-btn rounded  class="w-100 mt-2 main-btn" @click="Test">Test</v-btn>
         
                    </v-col>
                </v-row>
                <v-row>
                    <v-col  >
                 
                        <v-textarea
                            filled
                            auto-grow
                            label="Token"
                            rows="4"
                            row-height="30"
                            shaped
                            v-bind:value="token"
                        >
                        </v-textarea>
                    
                    </v-col>
                   
                </v-row>
                <v-row>
                    <v-col  class="pl-10 pr-10">
                 
                        <v-btn rounded  class="w-100 mt-2 main-btn" @click="getToken">Get Token</v-btn>
         
                    </v-col>
                </v-row>
                 <v-row>
                    <v-col  class="pl-10 pr-10">
                 
                        <v-btn rounded  class="w-100 mt-2 main-btn" @click="removeToken">Remove Token</v-btn>
         
                    </v-col>
                </v-row>
                <v-row>
                    <v-col  class="pl-10 pr-10">
                 
                        <v-btn rounded  class="w-100 mt-2 main-btn" @click="getToPage">Go To Page (Need Autorization)</v-btn>
         
                    </v-col>
                </v-row>
            </v-container>

        </v-main>

    </div>
</template>

<script>


export default {
    data() {
        return {

            liffId: process.env.LIFF_ID_REGISTER,
            webhook: process.env.WEBHOOK_API_URI,
            lineIdToken:'',
            userId1:'',
            userId2:'',
            token:'',

          
        }
    },
    async mounted () {


        await liff.init({
            liffId: this.liffId,

        }).then( ()=>{
            // liff.login()
            this.lineIdToken=liff.getIDToken()
        });


        
    },

    methods: {

        async  Test() {

                const data={ "lineIdToken":this.lineIdToken}

                let result=await this.$axios.post(`${this.webhook}/user_tokenid`,data)
    
                if (result.data.userId1!==''){
                    this.userId1=result.data.userId1
                    this.userId2=result.data.userId2
                }else{
                    liff.login()
                }
                                
        }, 

        async  getToken(){
            const profile=await liff.getProfile()

            let user={"userId":profile.userId,"name":profile.displayName}
            let result = await this.$axios.post(`${this.webhook}/auth/user`,user)
            this.token=result.data.accessToken

            await localStorage.setItem('accessToken', result.data.accessToken)
            //Code - get Token and save to local storage

            //Code - get Token and save to local storage

        },

        async  removeToken(){

            //Code - remove token from local storage
            this.token=''
            await localStorage.removeItem('accessToken')

        },

        getToPage(){
            this.$router.push({name: 'test2'})
        }

    },

};
</script>

<style>

</style>

