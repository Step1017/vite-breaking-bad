<script>
    import axios from 'axios';
    import {store} from '../store';
    export default {
        name:'AppMain',
        data() {
            return {
                store,
                archetypeChoice: '',
            };
        },

        methods: {
            getCards() {
                axios.get('https://db.ygoprodeck.com/api/v7/cardinfo.php').then((response) => {
                this.store.cards = response.data.data.slice(0,50);
                this.store.loading = true;
            });
            },

            getArchetypes() {
                axios.get('https://db.ygoprodeck.com/api/v7/archetypes.php').then((response) => {
                this.store.archetypes = response.data;
                });
            },

            filteredCards () {
                if (this.archetypeChoice != '') {
                    axios.get('https://db.ygoprodeck.com/api/v7/cardinfo.php', {
                        params: {
                            archetype: this.archetypeChoice
                        }
                    }).then((response) => {
                        this.store.cards = response.data.data.slice(0,50);
                    });
                } 
                else {
                    this.getCards ();
                }
            }
        },

        created() {
            this.getCards();
            this.getArchetypes();
        },
    };
</script>

<template>
    <main class="py-3">
        <div class="container" v-if="store.loading">
            <div class="mb-3">
                <select class="px-5 py-1 border rounded form-group col-md-3" name="filter" v-model="archetypeChoice" @change="filteredCards()">
                    <option value="" selected> 
                        Choose an Archetype
                    </option>
                    <option v-for="archetype in store.archetypes" :value="archetype.archetype_name">
                        {{ archetype.archetype_name }}
                    </option>
                </select>
            </div>
            <div class="p-5 bg-light">
                <div class="row bg-dark py-3 text-light fw-bold">
                    <p class="m-0">Found {{store.cards.length}} cards</p>
                </div>
                <div class="row row-cols-5 justify-content-between">
                    <div class="col text-center g-4"
                    v-for="card in store.cards">
                        <div class="card h-100">
                            <img class="img-fluid" :src="card.card_images[0].image_url_small" :alt="card.name">
                            <p class="my-3 text-light fw-bold"> {{card.name}}</p>
                            <p class="m-0">{{card.type}}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div v-else class="d-flex justify-content-center align-items-center py-5 bg-light ">
            <h2 class="me-3 text-dark">
                LOADING...
            </h2>
            <div class="spinner-border text-dark" role="status"></div>
        </div>        
    </main>
</template>

<style lang="scss" scoped>
    main {
        background-color: $ygo-orange;

        .card {
            background-color: $ygo-orange;
        }
    }
</style>