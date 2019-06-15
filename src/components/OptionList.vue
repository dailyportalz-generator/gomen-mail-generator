<template>
    <form @submit.prevent="handleSubmit" class="select-question">
        <p><strong>{{ message }}</strong></p>
        <template v-for="(option, i) in options">
            <p>
                <label>
                    <input type="radio" :value="i" v-model="selectedValue">
                    {{ option }}
                </label>
            </p>
        </template>
        <button>次へ</button>
    </form>
</template>

<script>
    export default {
        props: {
            message: String,
            options: Array,
            value: Number
        },
        data() {
            return {
                selectedValue: this.value
            }
        },
        methods: {
            handleSubmit() {
                this.$emit('input', this.selectedValue)
                this.$emit('next')
            }
        }
    }
</script>

<style scoped>
    strong {
        color: #FF6600;
    }
</style>