<template>
    <div class="mobile-config">
        <Form ref="formInline">
            <div  v-for="(item,key) in rCom" :key="key">
                <component :is="item.components.name" :configObj="configObj" ref="childData" :configNme="item.configNme" :key="key" @getConfig="getConfig" :index="activeIndex" :num="item.num"></component>
            </div>
            <rightBtn :activeIndex="activeIndex" :configObj="configObj"></rightBtn>
        </Form>
    </div>
</template>

<script>
    import toolCom from '@/components/mobileConfigRight/index.js'
    import rightBtn from '@/components/rightBtn/index.vue';
    import { mapMutations } from 'vuex'
    export default {
        name: 'c_home_service',
        componentsName: 'home_service',
        cname: '在线客服',
        props: {
            activeIndex: {
                type: null
            },
            num: {
                type: null
            },
            index: {
                type: null
            }
        },
        components: {
            ...toolCom,
            rightBtn
        },
        data () {
            return {
                hotIndex: 1,
                configObj: {}, // 配置对象
                rCom: [
                    {
                        components: toolCom.c_upload_img,
                        configNme: 'logoConfig'
                    },
                    {
                        components: toolCom.c_slider,
                        configNme: 'topConfig'
                    }
                    // toolCom.c_tab,
                    // toolCom.c_hot_word,
                    // toolCom.c_bg_color,
                    // toolCom.c_txt_tab,
                    // toolCom.c_slider,
                ] // 当前页面组件
            }
        },
        watch: {
            num (nVal) {
                // debugger;
                let value = JSON.parse(JSON.stringify(this.$store.state.admin.mobildConfig.defaultArray[nVal]))
                this.configObj = value;
            },
            configObj: {
                handler (nVal, oVal) {
                    this.$store.commit('admin/mobildConfig/UPDATEARR', { num: this.num, val: nVal });
                },
                deep: true
            }
        },
        mounted () {
            this.$nextTick(() => {
                let value = JSON.parse(JSON.stringify(this.$store.state.admin.mobildConfig.defaultArray[this.num]))
                this.configObj = value;
            })
        },
        methods: {
            // 获取组件参数
            getConfig (data) {
            }
        }
    }
</script>

<style scoped lang="stylus">
    .title-tips
        padding-bottom 10px
        font-size 14px
        color #333
        span
            margin-right 14px
            color #999
</style>
