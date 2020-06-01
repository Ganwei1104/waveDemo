<template>
    <div>
        <div>子组件inject页面</div>
        <div>
            <p>来自父组件的传参：{{index}}</p>
        </div>
        <three></three>
    </div>
</template>

<script>
import three from './three'
export default {
    data() {
        return {

        };
    },
    inject: ['index'] ,
    computed: {

    },
    created() {

    },
    methods: {

    },
    components: {
        three
    },
};
</script>

<style scoped lang="scss">

</style>
