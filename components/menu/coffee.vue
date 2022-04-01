<template>
  
    <v-card
        class="mb-5 pt-0 pb-0 pl-0 pr-0 rounded-lg"
        outlined elevation="3"
        
    >

        <v-img                                          
            v-bind:src="coffee.imageUrl"
            max-height="230"
            contain
            class="grey darken-4"
            lazy-src="https://raw.githubusercontent.com/kesinee-bo/sp01/master/Coffee/lazy_load_coffee.jpg"
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


        <v-card-text  class="pl-0 pr-0 pt-0 pb-0">
            <v-row align="center" justify="center" class="pl-2 pr-2">

                <v-col cols="9">
                    <div class="font-weight-black text-h6 text-left" style="color:black">
                        {{coffee.name}}
                    </div>
                </v-col>
                <v-col>
                    <div class="font-weight-black text-right darken-4 brown--text" >
                        {{coffee.price}} ฿
                    </div>
                </v-col>
            </v-row>

            
            <v-chip-group
                v-model="coffee.type"
                active-class="brown lighten-1 white--text"
                class="pl-2 pr-2"
        
            >
                <v-chip>Hot</v-chip>
                <v-chip>Ice</v-chip>
    
            </v-chip-group>
            <v-divider class="mx-4"></v-divider>
            <div class="font-weight-black text-subtitle-1 text-left pl-2 pr-2" style="color:black">
                Sweet
            </div>
            <v-chip-group
                v-model="coffee.sweet"
                active-class="brown lighten-1 white--text"
                class="pl-2 pr-2"
            >
                <v-chip>150%</v-chip>
                <v-chip>100%</v-chip>
                <v-chip>50%</v-chip>
                <v-chip>0%</v-chip>
            </v-chip-group>
            
            <v-container class="text-center pl-0 pr-0 pt-2 pb-2 blue-grey lighten-5" >
                <v-row  class="pl-0 pr-0" align="center"  justify="center">

                    <v-col align="left" >
                        <div v-if="coffee.qty>0">
                            <v-chip
                            class="mx-2 brown lighten-5"
                            color="brown"
                            dark
                            rounded
                            outlined
                            >
                                
                                <v-icon >
                                    mdi-coffee
                                </v-icon>
                                &nbsp;
                                {{coffee.qty}}
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
                                v-bind:disabled="btnstatus"
                                color="red"
                                v-bind:dark="!btnstatus"
                                @click="deleteFromCart(coffee)"
                                v-if="coffee.qty>0"
                            >
                                <v-icon dark>
                                    mdi-delete
                                </v-icon> 
                            </v-btn>
                            <v-btn
                                x-small
                                rounded
                                fab
                                v-bind:dark="!btnstatus"
                                class="mx-2 font-weight-regular"
                                v-bind:disabled="btnstatus"
                                color="brown darken-3"
                                
                                @click="addToCart(coffee)"
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
    name: 'Coffee',
    props: ["coffee","btnstatus"],

    data() {
        return {
            
        };
    },

    mounted() {

    },

    methods: {
        async addToCart(item){
            this.$emit('add:coffee', item);
            
        },
        async deleteFromCart(item){

            this.$emit('delete:coffee', item);

        },
    },
};
</script>

<style scoped>

</style>