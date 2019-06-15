<template>
    <div class="waku">
        <div class="waku_main">
            <img src="@/assets/waku_up.gif" alt>
            <div style="display: flex;align-items: stretch">
                <img src="@/assets/waku_left.gif" alt>
                <div class="waku__message">
                    <p v-if="step === 0">ここに文面が表示されます</p>
                    <p v-if="step > 0">{{ atena }} 様</p>
                    <template v-for="(result, i) in this.results">
                        <p>{{ config[i].messages[result] }}</p>
                    </template>
                </div>
                <img src="@/assets/waku_left.gif" alt>
            </div>
            <img src="@/assets/waku_down.gif" alt>
        </div>
        <template v-if="step >= 7">
            <p class="waku_build">
                <button type="button" @click="handleClickRestart">もう一度つくる</button>
            </p>
            <p class="waku_share">
                <input type="text" class="waku_shareurl" readonly :value="shareUrl">
                <a target="_blank" :href="twitterUrl">つぶやく</a>
            </p>
        </template>
    </div>
</template>

<script>
    import { stringify } from 'querystring'

    export default {
        props: {
            step: Number,
            atena: String,
            results: Array,
            config: Array
        },
        computed: {
            shareUrl() {
                const queries = {
                    atena: this.atena,
                    q1: this.results[0],
                    q2: this.results[1],
                    q3: this.results[2],
                    q4: this.results[3],
                    q5: this.results[4],
                    share: 1
                }
                return `https://dailyportalz.jp/kiji/owabi-mail-generator?${stringify(queries)}`
            },
            twitterUrl() {
                return `https://twitter.com/intent/tweet?text=ごめんなさいメールジェネレーター&url=${encodeURIComponent(
                    this.shareUrl
                )}`
            }
        },
        methods: {
            handleClickRestart() {
                location.href = location.href.replace(location.search, '')
            }
        }
    }
</script>

<style scoped>
    img {
        user-select: none;
    }

    .waku_main {
        font-size: 0;
    }

    .waku__message {
        padding: 16px;
        font-size: 12px;
        flex: 1;
        min-height: 150px;
    }

    .waku__message p:first-child {
        margin-top: 0;
    }

    .waku_build {
        text-align: right;
    }

    .waku_share {
        font-size: 12px;
    }

    .waku_shareurl {
        width: 100%;
    }

    .waku_share {
        display: flex;
        align-items: center;
    }

    input {
        margin: 4px 10px 4px 0;
        flex: 1;
    }
</style>