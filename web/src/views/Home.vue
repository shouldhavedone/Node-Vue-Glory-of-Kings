<template>
    <div>
        <swiper :options="swiperOption">

            <swiper-slide v-for="(ad, i) in adCats.items" :key="i">
                <a :href="ad.url" target="_blank">
                    <img class="w-100" :src="ad.image" :alt="ad.image">
                </a>
            </swiper-slide>

            <div class="swiper-pagination pagination-home text-right px-3 pb-1" slot="pagination"></div>
        </swiper>
        <!-- end of swiper -->

        <div class="nav-icons bg-white mt-3 text-center pt-3 text-dark-1">
            <div class="d-flex flex-wrap">
                <div class="nav-item mb-3">
                    <i class="sprite sprite-news"></i>
                    <div class="py-2">爆料站</div>
                </div>
                <div class="nav-item mb-3">
                    <i class="sprite sprite-story"></i>
                    <div class="py-2">故事站</div>
                </div>
                <div class="nav-item mb-3">
                    <i class="sprite sprite-shopping"></i>
                    <div class="py-2">周边商城</div>
                </div>
                <div class="nav-item mb-3">
                    <i class="sprite sprite-experiential"></i>
                    <div class="py-2">体验服</div>
                </div>
                <div class="nav-item mb-3">
                    <i class="sprite sprite-people"></i>
                    <div class="py-2">新人专区</div>
                </div>
                <div class="nav-item mb-3">
                    <i class="sprite sprite-inherit"></i>
                    <div class="py-2">荣耀·传承</div>
                </div>
                <div class="nav-item mb-3">
                    <i class="sprite sprite-colleaguces"></i>
                    <div class="py-2">同人社区</div>
                </div>
                <div class="nav-item mb-3">
                    <i class="sprite sprite-camp"></i>
                    <div class="py-2">王者营地</div>
                </div>
                <div class="nav-item mb-3">
                    <i class="sprite sprite-address"></i>
                    <div class="py-2">公众号</div>
                </div>
                <div class="nav-item mb-3">
                    <i class="sprite sprite-introduce"></i>
                    <div class="py-2">版本介绍</div>
                </div>
            </div>

            <div class="bg-light py-2 fs-sm">
                <i class="sprite sprite-arrow mr-1"></i>
                <span>收起</span>
            </div>
        </div>
        <!-- end of nav icons -->

        <m-list-card icon="menu" title="新闻资讯" :categories="newsCats">
            <template #items="{category}">
                <router-link
                        tag="div"
                        :to="`/articles/${news._id}`"
                        class="py-2 fs-lg d-flex"
                        v-for="(news, i) in category.newsList" :key="i">
                    <span class="text-info">[{{news.categoryName}}]</span>
                    <span class="px-2">|</span>
                    <span class="flex-1 text-dark-1 text-ellipsis pr-2">{{news.title}}</span>
                    <span class="text-grey-1 fs-sm">{{news.createdAt | date}}</span>
                </router-link>
            </template>
        </m-list-card>

        <m-list-card icon="toukui" title="英雄列表" :categories="heroCats">
            <template #items="{category}">
                <div class="d-flex flex-wrap" style="margin: 0 -0.5rem;">
                    <router-link
                            tag="div"
                            :to="`/heroes/${hero._id}`"
                            class="p-2 text-center"
                            style="width: 20%;"
                            v-for="(hero, i) in category.heroList" :key="i">
                        <img :src="hero.avatar" class="w-100">
                        <div>{{hero.name}}</div>
                    </router-link>
                </div>
            </template>
        </m-list-card>

        <m-card icon="vedio1" title="精彩视频"></m-card>

        <m-card icon="read" title="图文攻略"></m-card>
    </div>
</template>

<script>
    import dayjs from "dayjs";

    export default {
        filters: {
            date(val) {
                return dayjs(val).format("MM/DD");
            }
        },
        data() {
            return {
                swiperOption: {
                    pagination: {
                        el: ".pagination-home",
                    },
                    effect : 'coverflow',
                    loop: true,
                    autoplay: {
                        delay: 3000,
                        stopOnLastSlide: false,
                        disableOnInteraction: false
                    }
                },
                newsCats: [],
                heroCats: [],
                adCats:[],
            };
        },
        methods: {
            async fetchNewsCats() {
                const res = await this.$http.get("news/list");
                this.newsCats = res.data;
            },
            async fetchHeroCats() {
                const res = await this.$http.get("heroes/list");
                this.heroCats = res.data;
            },
            async fetchAdCats() {
                const res = await this.$http.get("ads/list");
                this.adCats = res.data[0];
            },

        },
        created() {
            this.fetchNewsCats();
            this.fetchHeroCats();
            this.fetchAdCats();
        },
    };
</script>

<style lang="scss">
    @import "../assets/scss/variables";

    .pagination-home {
        .swiper-pagination-bullet {
            opacity: 1;
            border-radius: 0.1538rem;
            background: map-get($colors, "white");

            &.swiper-pagination-bullet-active {
                background: map-get($colors, "info");
            }
        }
    }

    .nav-icons {
        border-top: 1px solid $border-color;
        border-bottom: 1px solid $border-color;

        .nav-item {
            width: 25%;
            border-right: 1px solid $border-color;

            &:nth-child(4n) {
                border-right: none;
            }
        }
    }
</style>
