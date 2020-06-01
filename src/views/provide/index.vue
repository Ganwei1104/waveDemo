<template>
    <div class="project">
        <el-button @click="goChild">前往子组件查看</el-button>
        <div class="show-inject" v-show="showChild">
            <inject></inject>
        </div>
    </div>
</template>

<script>
import inject from './inject'
export default {
    data() {
        return {
            showChild:false,
            custom: 'tom',
            custon: 'ton'
        };
    },
    provide: {
        index: '来自provide的数据'
    },
    computed: {

    },
    created() {

    },
    methods: {
        goChild(){
            this.showChild = !this.showChild;
        }
    },
    components: {
        inject
    },
};
</script>

<style scoped lang="scss">
.project{
    padding: 100px 50px;
    .show-inject{
        padding: 20px;
    }
}
</style>
