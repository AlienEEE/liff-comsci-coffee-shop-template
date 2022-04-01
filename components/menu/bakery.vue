<template>
    <v-card
        class="mb-5 pb-0 pl-0 pr-0 rounded-lg"
        outlined elevation="3"
    >
        

        <v-img
        v-bind:src="bakery.imageUrl"
        max-height="230"
        contain
        class="grey darken-4"
        lazy-src="https://raw.githubusercontent.com/kesinee-bo/sp01/master/Bakery/lazy_load_bakery.jpg"
        >
            <template v-slot:placeholder>
                <v-row
                class="fill-height ma-0"
                align="center"
                justify="center"
                >
                <v-progress-circular
                    indeterminate
                    color="grey lighten-5"
                ></v-progress-circular>
                </v-row>
            </template>
        
        </v-img>


        <v-card-text class="pl-0 pr-0 pt-0 pb-0">
            <v-row align="center" justify="center" class="pl-2 pr-2">

                <v-col cols="9">
                    <div class="font-weight-black text-h6 text-left" style="color:black">
                        {{bakery.name}}
                    </div>
                </v-col>
                <v-col>
                    <div class="font-weight-black text-right darken-4 brown--text" >
                        {{bakery.price}} ฿
                    </div>
                </v-col>
            </v-row>
            <v-container class="pl-0 pr-0 pt-2 pb-2 blue-grey lighten-5" fill-height fluid>

                <v-row  class="pl-3 pr-0" align="center"  justify="center">

                    <v-col align="left" >
                        <div v-if="bakery.qty>0">
                            <v-chip
                                class="mx-2 brown lighten-5"
                                color="brown"
                                dark
                                rounded
                                outlined
                                >                                                          
                                <v-icon >
                                    mdi-baguette 
                                </v-icon>
                                
                                &nbsp;
                                {{bakery.qty}}
                            </v-chip>
                        </div>
                        
                    </v-col>
                    <v-col align="right" >
                        <div class="pr-0">
                            <v-btn
                                x-small
                                rounded
                                fab
                                class="mx-2 font-weight-regular"
                                color="red"
                                v-bind:dark="!btnstatus"
                                @click="deleteFromCart(bakery)"
                                v-bind:disabled="btnstatus"
                                v-if="bakery.qty>0"
                            >
                                <v-icon dark>
                                    mdi-delete
                                </v-icon> 
                            </v-btn>
                            <v-btn
                                x-small
                                rounded
                                fab
                                class="mx-2 font-weight-regular"
                                color="brown darken-3"
                                v-bind:dark="!btnstatus"
                                @click="addToCart(bakery)"
                                v-bind:disabled="btnstatus"
                            >
                                <v-icon dark>
                                    mdi-basket-plus
                                </v-icon> 
                            </v-btn>
                            
                        </div>
                    </v-col>
                </v-row>
                
            </v-container>
        </v-card-text>

    </v-card>
</template>

<script>
export default {
    name: 'Bakery',
    props: ["bakery","btnstatus"],
    data() {
        return {
            
        };
    },

    mounted() {
        
    },

    methods: {
        async addToCart(item){
            this.$emit('add:bakery', item);
            
        },
        async deleteFromCart(item){

            this.$emit('delete:bakery', item);

        },
    },
};
</script>

<style scoped>

</style>