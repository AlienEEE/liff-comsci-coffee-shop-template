<template>
    <div style="overflow: hidden;">
        <v-app-bar  dense flat >
            <v-toolbar-title class="pa-0">Menu</v-toolbar-title>
        </v-app-bar>
        
        <v-main >

            <div :style="{visibility: overlay ? 'hidden' : 'visible'}">

                <v-container  class="pt-3 pb-5 pr-0 pl-0"  >
                    <v-row>
                        <v-col cols="12" class="pl-10 pr-10">
                            <v-tabs
                                class="pb-5"
                                v-model="tab"
                                centered
                                slider-color="black"
                                
                            >

                                <v-tab
                                    href="#coffee"
                                >
                                    Coffee
                                </v-tab>


                                <v-tab
                                    href="#bakery"
                                >
                                    Bakery
                                </v-tab>
                                
                            </v-tabs>

                            <v-tabs-items v-model="tab">
                                <v-tab-item
                                    value="coffee"
                                >
                                     <v-container fluid>
                                        <v-row dense>
                                            <v-col
                                                v-for="coffee in coffees"
                                                :key="coffee.coffeeId"                           
                                                cols="12"
                                                xs="12"
                                                sm="6"
                                                md="4"
                                                lg="3"
                                                xl="2"
                                            >

                                                <Coffee v-bind:coffee="coffee" v-bind:btnstatus="btnstatus" v-on:delete:coffee="deleteFromCart" v-on:add:coffee="addToCart" />

                                            </v-col>
                                        </v-row>
                                     </v-container>
                                     

                                   

  
                                </v-tab-item>
                                <v-tab-item
                                    value="bakery"
                                >


                                    <v-container fluid>
                                        <v-row dense>
                                            <v-col
                                                v-for="bakery in bakeries"
                                                :key="bakery.bakeryId"                           
                                                cols="12"
                                                xs="12"
                                                sm="6"
                                                md="4"
                                                lg="3"
                                                xl="2"
                                            >

                                                <Bakery v-bind:bakery="bakery" v-bind:btnstatus="btnstatus" v-on:delete:bakery="deleteFromCart" v-on:add:bakery="addToCart"  />

                                            </v-col>
                                        </v-row>
                                     </v-container>


                                  
                                </v-tab-item>
                            </v-tabs-items>



                        </v-col>
                    </v-row>
                    <v-row  justify="center" v-if="isInClient">
                        <v-col
                            cols="12"
                            xs="12"
                            sm="6"
                            md="4"
                            lg="3"
                            xl="2"
                        >
                            <v-container class="pt-0 pb-0 pr-10 pl-10 text-center">
                                <v-btn rounded v-bind:disabled="btnstatus" class="w-100  main-btn" @click="close">Close</v-btn>
                            </v-container>

                        </v-col>
                    </v-row>
                </v-container> 
                <v-snackbar
                    v-model="snackbar"
                    >
                    {{ text }}

                    <template v-slot:action="{ attrs }">
                        <v-btn
                        color="blue"
                        text
                        v-bind="attrs"
                        @click="snackbar = false"
                        >
                        Close
                        </v-btn>
                    </template>
                </v-snackbar>


            </div>
            <v-overlay
            :opacity="1"
            :value="overlay"
            >
                <v-progress-circular indeterminate size="64">
                    Loading
                </v-progress-circular>
            </v-overlay>



        </v-main>

    </div>
</template>

<script>
import Coffee from '../../components/menu/coffee'
import Bakery from '../../components/menu/bakery';
export default {
    components: {
        Coffee,
        Bakery,
    },
    data() {
        return {

            liffId: undefined,
            webhook:process.env.WEBHOOK_API_URI,
            
            lineId:'',
            lineName:'',

            
            pictureUrl:'',
            isInClient:false,
            
            coffees:'',
            bakeries:'',

            tab:'coffee',
            btnstatus:true,
            overlay:true,

            snackbar: false,
            text: `Added to cart!`,


          
        }
    },
    async  mounted () {

        (this.$route.query.tab !== undefined) ? this.tab=this.$route.query.tab : this.tab='coffee'
        

        if (this.tab==='coffee'){
            this.liffId=process.env.LIFF_ID_COFFEE
        }else{
            this.liffId=process.env.LIFF_ID_BAKERY
        }
        
        await liff.init({
            liffId: this.liffId,
        })

        await this.getUserProfile()
        this.isInClient=liff.isInClient()




        
        try {
                        
                let result=await this.$axios.get(`${this.webhook}/products/${this.lineId}`)
                
                if (!result.error){

                    this.coffees=result.data.data.coffees
                    this.bakeries=result.data.data.bakeries

                }

                this.overlay=false
                this.btnstatus=false
        


        }
        catch(err){
            this.text=err.message
            this.snackbar = true
            this.btnstatus=false
        } 
        
    },

    methods: {
         async  getUserProfile() {

            if (liff.isLoggedIn()){
                
                const profile=await liff.getProfile()

                this.lineId=profile.userId
                this.lineName=profile.displayName
                this.pictureUrl=profile.pictureUrl

                     
            }else{

                liff.login()

            }
                
        }, 


        async addToCart(item){

            try{

                this.btnstatus=true
                
                let data={"data":item,"lineId":this.lineId}
                await this.$axios.post(`${this.webhook}/cart`,data)
                item.qty++
                this.text=`${item.name} is added to cart !`
                this.snackbar = true
                this.btnstatus=false

            }catch(err){

                this.text=err.message
                this.snackbar = true
                this.btnstatus=false

            }
            
        },
        async deleteFromCart(item){

            try{

                this.btnstatus=true
                
                let data={data: {"data":item,"lineId":this.lineId}}
                await this.$axios.delete(`${this.webhook}/cart`,data)
                item.qty=0
                this.text=`${item.name} is deleted from cart !`
                this.snackbar = true
                this.btnstatus=false

            }catch(err){

                this.text=err.message
                this.snackbar = true
                this.btnstatus=false

            }

        },
        async close() {

            this.btnstatus=false
            if (liff.isInClient()){  
                await liff.sendMessages([{
                    type: "text",
                    text: "แสดงข้อมูลสินค้าในตระกร้า"
                }])
                await liff.closeWindow() 
            
            }   
        }, 
        async  logout() {
            
            await liff.logout()
            liff.login()
         
        }, 
    },

};
</script>

<style>
.v-tabs-slider-wrapper, .v-tab--active {
  color: black !important;
  font-weight:bolder
}

</style>