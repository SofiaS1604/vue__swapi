<template>
    <div class="page__main main__card">
        <cards-list v-if="this.result" :cardsListProps="this.result.results" :key="this.keyCount"/>
        <div class="main__buttons buttons" v-if="this.result">
            <button-navigation :class="{'buttons__item--active': this.result.previous}"
                               @click.native="clickButton('previous')">
                Previous
            </button-navigation>
            <button-navigation :class="{'buttons__item--active': this.result.next}" @click.native="clickButton('next')">
                Next
            </button-navigation>
        </div>
    </div>
</template>

<script>
    import CardsList from "../widgets/CardsList.vue";
    import ButtonNavigation from "../atoms/ButtonNavigation.vue";
    import axios from "axios";

    const componentsList = {};
    componentsList[CardsList.name] = CardsList;
    componentsList[ButtonNavigation.name] = ButtonNavigation;

    export default {
        name: "PageCards",
        components: componentsList,
        data() {
            return {
                pageCount: +this.$route.query.page,
                result: null,
                images: [],
                keyCount: 1,
            }
        },
        methods: {
            clickButton(type) {
                this.pageCount += type === 'next' && this.result.next ? 1 : 0;
                this.pageCount -= type === 'previous' && this.result.previous ? 1 : 0;

                this.$router.replace({
                    query: {
                        page: this.pageCount
                    }
                }).catch(() => {
                });
            },

            async getData() {
                return new Promise(async resolve => {
                    let res = await axios.get(`https://swapi.dev/api${this.$route.path}?page=${this.pageCount}`);
                    resolve(await res);
                });
            },

            async getPeople() {
                this.result = await this.getData().then(res => this.result = res.data);
                this.keyCount = this.pageCount
            },
        },
        watch: {
            pageCount() {
                this.getPeople();
            }
        },
        mounted() {
            this.getPeople();
        },
    }
</script>