<template>
    <div class="ww-text-popup-html ww-scroll-bar">
        <prism-editor class="editor" :code="code" language="html" :lineNumbers="true" @change="setResult($event)"></prism-editor>
    </div>
</template>


<script>
import "prismjs";
import "prismjs/themes/prism.css";
import VuePrismEditor from "vue-prism-editor";
import "vue-prism-editor/dist/VuePrismEditor.css"; // import the styles
Vue.component("prism-editor", VuePrismEditor);

export default {
    name: "wwTextPopupHtml",
    props: {
        options: Object,
    },
    data() {
        return {
            code: ''
        }
    },
    computed: {
    },
    watch: {
    },
    methods: {
        init() {
            this.code = this.options.data.html

            this.setResult(this.code)
        },
        setResult(code) {
            this.options.result.html = code;
        },

        beforeNext() {
        }
    },
    created() {
        this.init();
    },
    mounted() {
    },
    beforeDestroyed() {

    }
};
</script>

<style scoped lang="scss">
.ww-text-popup-html {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;

    .editor {
        width: 90%;
        height: 400px;
    }
}
</style>