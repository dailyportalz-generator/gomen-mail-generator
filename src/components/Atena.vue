<template>
    <form @submit.prevent="handleSubmit" class="input-question">
        <p><strong>step 1</strong> あて名を入れてください</p>
        <p><input v-model="atena" type="text" size="40" required> さま</p>
        <button>次へ</button>
    </form>
</template>

<script>
    export default {
        data() {
            return {
                atena: ''
            }
        },
        methods: {
            handleSubmit() {
                this.$emit('setAtena', {
                    atena: this.atena
                })
            }
        }
    }
</script>

<style scoped>
    strong {
        color: #FF6600;
    }
</style>