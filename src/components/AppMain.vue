<script>
    import {store} from '../store.js';
    export default {
        name:'AppMain',
        data() {
            return {
                store,
            };
        },
    };
</script>

<template>
    <main class="py-3">
        <div class="container">
            <div class="mb-3">
                <select name="filter">
                    <option value="Alien" selected>
                        Alien
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