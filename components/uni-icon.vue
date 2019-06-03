<template>
    <view class="uni-icon" :class="['uni-icon-'+type]" :style="{color:color,'font-size':fontSize}"></view>
</template>

<script>
    export default {
        props: {
            type: String,
            color: String,
            size: [Number, String]
        },
        computed: {
            fontSize() {
                return `${this.size}px`
            }
        }
    }
</script>

