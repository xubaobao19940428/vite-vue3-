<template>
    <div class="editor-container">
        <div ref="reditor"></div>
    </div>
</template>
<script>
import { defineComponent, ref, onMounted, toRefs, watch } from 'vue';
import E from 'wangeditor';

export default defineComponent({
    name: 'RichEditor',
    props: {
        modelValue: {
            type: String,
            default: '',
        },
    },
    emits: ['update:modelValue'],
    setup(props, { emit }) {
        const reditor = ref(); // 富文本引用
        const { modelValue } = toRefs(props);
        let editor = E;

        onMounted(() => {
            editor = new E(reditor.value);
            // 配置 onchange 回调函数
            editor.config.onchange = newHtml => {
                emit('update:modelValue', newHtml);
            };

            // 配置触发 onchange 的时间频率，默认为 200ms
            editor.config.onchangeTimeout = 500; // 修改为 500ms
            editor.create();

            // 使用 "父组件或本组件" 的默认数据 进行初始化
            editor.txt.html(modelValue.value);
        });
        watch(modelValue, newValue => {
            // 监听父组件数据变化 (例如：加载网络请求返回的数据)
            editor.txt.html(newValue);
        });
        return {
            reditor,
        };
    },
    methods: {
        btn1() {
            console.log(this.reditor);
            // var json = editor.txt.getJSON(); // 获取 JSON 格式的内容
            // var jsonStr = JSON.stringify(json);
            // console.log(json);
            // console.log(jsonStr);
            // console.log(editor.txt.html()); //获取内容
        },
    },
});
</script>
<style lang="scss" scoped>
.editor-container {
    :deep(.w-e-text-container) {
        text-align: left;
    }
}
</style>
