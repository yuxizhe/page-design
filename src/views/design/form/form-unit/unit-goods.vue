<template>
    <div class="form-item col-1">
        <label>商品数据</label>
        <div>
            <template v-if="!is_data_set">
                <a-button
                    class="button-add"
                    size="large"
                    type="primary"
                    @click="handle_open_dialog">选择商品数据</a-button>
            </template>
            <!-- 编辑区域 -->
            <template v-else>
                <div class="form-goods">
                    <a-input
                        :disabled="true"
                        :value="tips"
                        size="large"
                        :data-id="current_value" />

                    <a-button
                        class="button-edit"
                        size="large"
                        type="primary"
                        @click="handle_open_dialog">查看</a-button>
                </div>
            </template>
        </div>
        <!-- 弹窗 -->
        <goods-source-manager
            ref="goodsSourceManager"
            :value.sync="current_value"
            @confirm="handle_dialog_confirm" />

    </div>
</template>

<script>

// 商品数据弹窗
import goodsSourceManager from '../../more/goods-source-manager/index';


// Main code
export default {
    props: ['value', 'config'],

    components: {
        goodsSourceManager,
    },

    data () {
        return {
            current_value: this.value
        }
    },

    computed: {
        // 当前组件ID
        component_id () {
            return this.$store.state.design.selected_id;
        },
        // 是否已经设置数据
        is_data_set () {
            return this.value.length > 0;
        },
        // 提示文案
        tips () {
            return `已选择数量: ${this.value.length}个`;
        }
    },

    methods: {
        /**
         * 打开商品数据配置的弹窗
         */
        handle_open_dialog () {
            this.$refs.goodsSourceManager.show(this.value);
        },

        /**
         * 商品数据配置弹窗 - 确认回调
         * @param {Array} list 商品数据列表
         */
        handle_dialog_confirm (list) {
            this.$emit('input', list);
        }
    }
}
</script>

<style lang="less" scoped>
// 选择按钮
.button-add {
    width: 100%;
    font-size: 14px;
}
// 编辑区域
.form-goods {
    display: flex;
    flex-flow: row nowrap;

    .ant-input {
        flex-shrink: 1;
        margin-right: 8px;
        text-overflow: ellipsis;
    }
    .button-edit {
        font-size: 14px;
        flex-shrink: 0;
    }
}
</style>
