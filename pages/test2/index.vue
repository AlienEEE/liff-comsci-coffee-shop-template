<template>
    <div style="overflow: hidden;">
        <v-app-bar  dense flat >
            <v-toolbar-title class="pa-0 mb-3">Test</v-toolbar-title>
        </v-app-bar>
        <v-main >


            <v-container  class="pt-10 pb-0 pr-0 pl-0"  >
                <v-row>
                    <v-col  >
                 
                        <div class="text-subtitle text-center">Autorization Status</div>
                    
                    </v-col>
                   
                </v-row>
                <v-row>
                   
                    <v-col  >
                 
                        <div class="text-subtitle text-center  red">{{status}}</div>
         
                    </v-col>
                </v-row>
                <v-row v-if="(status=='pass') ? true : false">
                   
                    <v-col  >
                 
                        <div class="text-subtitle text-center  yellow">{{text_result}}</div>
         
                    </v-col>
                </v-row>
                <v-row v-if="(status=='pass') ? true : false">
                    <v-col  class="pl-10 pr-10">
                 
                        <v-btn rounded  class="w-100 mt-2 main-btn"  @click="getData">Get Data</v-btn>
         
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


            webhook: process.env.WEBHOOK_API_URI,
            status:'',
            accessToken:'',
            text_result:'',


          
        }
    },
    async mounted () {


        //Code - Read Token from local storage
        this.accessToken= await localStorage.getItem('accessToken')
        let data={"accessToken":this.accessToken}
        //Code - verify Token 
        let result =await this.$axios.post(`${this.webhook}/auth/verify_token`,data)
        this.status=result.data.verify_status
    
        
    },

    methods: {
        async getData(){
            
            //Code - declare header
            let headers={ headers: {"Authorization" : `bearer ${this.accessToken}`}}
            //Code - get test data 
            let result=await this.$axios.get(`${this.webhook}/test`,headers)
            if (!result.error){

                this.text_result=result.data.message


            }
        }


    },

};
</script>

<style>

</style>

