<template>
    <div id="app">
        <div class="section" v-if="step < 99">
            <Atena
                    v-if="step === 0"
                    @setAtena="setAtena"
                    key="atena"
            />
            <OptionList
                    v-if="step === 1"
                    v-model="results[0]"
                    :message="getOptionListConfigByKey(0).message"
                    :options="getOptionListConfigByKey(0).options"
                    @next="next"
                    key="q1"
            />
            <OptionList
                    v-if="step === 2"
                    v-model="results[1]"
                    :message="getOptionListConfigByKey(1).message"
                    :options="getOptionListConfigByKey(1).options"
                    @next="next"
                    key="q2"
            />
            <OptionList
                    v-if="step === 3"
                    v-model="results[2]"
                    :message="getOptionListConfigByKey(2).message"
                    :options="getOptionListConfigByKey(2).options"
                    @next="next"
                    key="q3"
            />
            <OptionList
                    v-if="step === 4"
                    v-model="results[3]"
                    :message="getOptionListConfigByKey(3).message"
                    :options="getOptionListConfigByKey(3).options"
                    @next="next"
                    key="q4"
            />
            <OptionList
                    v-if="step === 5"
                    v-model="results[4]"
                    :message="getOptionListConfigByKey(4).message"
                    :options="getOptionListConfigByKey(4).options"
                    @next="finish"
                    key="q5"
            />
        </div>
        <Waku :step="step" :atena="atena" :results="results" :config="getOtpionListConfig" key="waku"/>
    </div>
</template>

<script>
    import Atena from './components/Atena.vue'
    import OptionList from './components/OptionList.vue'
    import Waku from './components/Waku.vue'
    import { parse } from 'querystring'

    export default {
        name: 'app',
        components: {
            Atena, OptionList, Waku
        },
        data() {
            return {
                step: 0,
                atena: '',
                results: []
            }
        },
        computed: {
            getOtpionListConfig() {
                return [
                    {
                        message: 'どう書き出しましょうか',
                        options: [
                            'おそるおそるに',
                            'もったいぶって',
                            '子どもっぽく',
                            'いきなり泣いちゃおう',
                            '明るく',
                            'バカっぽく、そして古く'
                        ],
                        messages: [
                            'この度はとんだ事をいたしまして…。',
                            '今日という今日は申し上げなければならないことがあります。昨日の件です。',
                            'こんにちは。ごめんなさいのことがあるの。このまえのことです。',
                            '昨日反省してお風呂で泣きました。今回の件のことです。',
                            'ゴメス！このまえはまったくゴメス！',
                            'いやー、メンゴメンゴ。このまえは超メンゴ～'
                        ]
                    },
                    {
                        message: 'まずはお詫び本文',
                        options: [
                            'ちょっと難しい言葉を使って',
                            '未来志向で謝る',
                            'ふつうに、でも感情を込めて',
                            '時代劇風に',
                            '一歩引いて',
                            'カジュアルに'
                        ],
                        messages: [
                            '申し開きの言葉もございません。',
                            '孫の代まで申し訳ありません。',
                            'ほんっ……とうに申し訳ありません。',
                            'お詫びつかまつる。',
                            '月並みなお詫びしかできませんが、申し訳ありませんでした。',
                            'ごめんよぅ'
                        ]
                    },
                    {
                        message: '言い訳しておきますか',
                        options: [
                            '全て認めておく',
                            '逆ギレする',
                            '天気のせいにする',
                            '二重人格ということにしておく',
                            '人のせいにしておく',
                            '異常気象の一環に'
                        ],
                        messages: [
                            '完全に私のミスです。申し訳ございません。うっかりしておりました。',
                            '全部私が悪いんだ！煮ろ！焼け！',
                            'すみません、向かい風だったものですから。',
                            '私、血迷ってしまいました（私のなかの別の私のしわざです）。',
                            'でもね、悪いのはすべてあの人です。いまはまだあの人としか言えませんが時が来たら分かるはず。',
                            'しかしエルニーニョがこのようなことも引き起こすとは思いませんでした。私もびっくりです。'
                        ]
                    },
                    {
                        message: 'しかしもういちど謝っておこう',
                        options: [
                            'おおげさに謝る',
                            '長く書いてごまかす',
                            '要はごめんだけど一歩引いて',
                            '泣きおとし',
                            'あわよくば笑ってごまかせたら',
                            'こどものように'
                        ],
                        messages: [
                            '重ね重ね申し訳ありませんでした。なんでしたら体でお返しします！',
                            'このたびはご不快な思いをさせ、お手を煩わせてしまいましたこと、心からお詫び申し上げます。至らぬ者ではございますが、今後ともご指導、ご教示賜りたく存じます。何卒よろしくお願い申し上げます。まことに申し訳ございません。重ねてお詫び申し上げます。',
                            'もともと徳のない私ですが、不徳の致す限りです。',
                            'ごめんなさい、私たち、もう終わりかしら…。',
                            'これから私のことは抜け作と呼んでいただいて結構です。',
                            'ごめんなさいもうしませんゆるしてください。'
                        ]
                    },
                    {
                        message: '締めましょう',
                        options: [
                            'ノーマル',
                            '用意周到に',
                            'ひたすら低姿勢で',
                            '金で済ます',
                            '告白しちゃう',
                            '変化球で'
                        ],
                        messages: [
                            '今後、二度とこのようなことの無い様に厳重に注意させて頂きます。',
                            'お詫びのしるしに顛末書を添付致しております。',
                            'わたくしめのような愚かな人間にお付き合いいただき、まことにありがとうございました。',
                            '今度、焼肉おごるから！',
                            '実は…好きなんです。',
                            'このご恩は忘れるまで忘れません'
                        ]
                    }
                ]
            }
        },
        methods: {
            setAtena(atena) {
                this.atena = atena.atena
                this.next()
            },
            next() {
                this.step++;
            },
            finish() {
                this.step = 99;
            },
            getOptionListConfigByKey(i) {
                return this.getOtpionListConfig[i]
            }
        },
        mounted() {
            const params = parse(location.search.replace('?', ''))
            const isValid = ['atena', 'q1', 'q2', 'q3', 'q4', 'q5'].every((val) => {
                if (!params[val]) {
                    return false
                }
                if (val !== 'atena' && Number(parseFloat(params[val])) === params[val]) {
                    return false
                }
                return true
            })
            console.log(isValid)
            if (isValid) {
                let results = []
                results.push(parseInt(params.q1))
                results.push(parseInt(params.q2))
                results.push(parseInt(params.q3))
                results.push(parseInt(params.q4))
                results.push(parseInt(params.q5))
                this.results = results
                this.atena = params.atena
                this.step = 99
            }
        }
    }
</script>

<style>
    #app {
        width: 526px;
        margin: 0 auto;
        font-family: メイリオ, Meiryo, "ヒラギノ角ゴ Pro W3", "Hiragino Kaku Gothic Pro", "ＭＳ Ｐゴシック", sans-serif !important;
        line-height: 1.8;
    }

    .section {
        border: 1px solid #FF6600;
        padding: 10px;
        margin-bottom: 20px;
    }
</style>
