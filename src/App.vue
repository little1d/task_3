<script setup>
import { marked } from 'marked'
import { ref, onMounted, nextTick ,computed} from 'vue'
import * as emoji from 'node-emoji'

console.log(emoji.emojify('I :heart: :coffee:!'))
const showWriteBox = ref(true)
const showPreviewBox = ref(false)
const writeBoxRef = ref(null)
const markdownContent = ref('')


const parseMarkdownContent = computed(()=>{
    return emoji.emojify(markdownContent.value)
})

function toggleWriteBox() {
    showWriteBox.value = true
    showPreviewBox.value = false
    nextTick(() => {
        if (writeBoxRef.value) {
            writeBoxRef.value.focus()
        }
    })
}

function togglePreviewBox() {
    showWriteBox.value = false
    showPreviewBox.value = true
    nextTick(() => {
        if (showPreviewBox) {   bvcxz
            // Prism.highlightAll()
        }
    })
}

// 文本框根据文字调整大小
const adjustTextareaHeight=()=>{
    const textarea = writeBoxRef.value;
    textarea.style.height = "auto";
    textarea.style.height = `${textarea.scrollHeight}px`
}

onMounted(() => {
    if (showWriteBox.value) {
        writeBoxRef.value.focus()
    }
})
</script>

<template>
    <div class="container">
        <div class="header">
            <div class="ll-box"></div>
            <div class="left-box" :class="{ 'selected-box': showWriteBox, 'unselected-box': showPreviewBox }">
                <button class="left-button" @click="toggleWriteBox"
                    :class="{ 'selected-font': showPreviewBox }">write</button>
            </div>
            <div class="right-box" :class="{ 'selected-box': showPreviewBox, 'unselected-box': showWriteBox }">
                <button class="right-button" @click="togglePreviewBox"
                    :class="{ 'selected-font': showWriteBox }">preview</button>
            </div>
            <div class="rr-box"></div>
        </div>

        <div class="main">
            <textarea v-model="markdownContent" ref="writeBoxRef" v-if="showWriteBox" placeholder="Leave a comment"
                class="google-font write-box" cols="90" rows="1" @input="adjustTextareaHeight"></textarea>
            <div v-if="showPreviewBox" class="preview-box" >
                <pre class="language-markdown google-font" v-html="markdownContent ? marked(parseMarkdownContent) : 'Nothing to preview'"></pre>
            </div>
        </div>
        <hr>
        <div class="footer google-font">Remember, contributions to this repository should follow our <a href="#">Github
                Community Guidelines.</a></div>
    </div>
</template>

<style scoped>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.language-markdown{
    padding: 0;
    margin: 0;
}

.container {
    width: 100%;
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: space-between;
    background-color: #ffffff;
    border: 1px solid #d0d7de;
    border-radius: 6px;
    padding: 1rem;
    margin: 1rem;
    /* gap: 1rem; */
}

.container::before,
.container::after {
    content: "";
    position: absolute;
    top: 10px;
    left: -25px;
    border-width: 12px;
    border-style: solid;
}

.container::before {
    border-color: transparent #d0d7de transparent transparent;
}

.container::after {
    top: 10px;
    left: -23px;
    border-color: transparent #ffffff transparent transparent;
}

.header {
    width: 100%;
    height: 50px;
    display: flex;
    padding-bottom: 1rem;
}

.left-box {
    flex: 20%;
    background-color: #ffffff;
}

.right-box {
    flex: 20%;
    background-color: #ffffff;
}

.selected-font {
    color: #999999;
}

.selected-box {
    border: 1px solid #d0d7de;
    border-top-left-radius: 5px;
    border-top-right-radius: 5px;
    border-bottom: none;
}

.unselected-box {
    border-bottom: 1px solid #d0d7de;
}

.write-box {
    outline: none;
    border: 1px solid #d0d7de;
    resize: none; /* 禁止文本框的大小调整 */
    overflow: hidden; /* 隐藏文本框的滚动条 */
    height: auto; /* 允许文本框的高度根据内同自动调整 */
    min-height: 300px;
}

.write-box:focus {
    border: 3px solid #218bff;
    border-radius: 6px;
}

.write-box:not(:focus) {
    background-color: #f6f8fa;
}

.preview-box {
    background-color: #fff;
    resize: none; /* 禁止文本框的大小调整 */
    overflow: hidden; /* 隐藏文本框的滚动条 */
    height: auto; /* 允许文本框的高度根据内同自动调整 */
    min-height: 300px;
}

.ll-box {
    flex: 2%;
    border-bottom: 1px solid #d0d7de;

}

.rr-box {
    flex: 60%;
    border-bottom: 1px solid #d0d7de;
}

.footer {
    font-size: small;
}

.google-font {
    font-family: 'Poppins', sans-serif;
}

button {
    border: none;
    background-color: transparent;
    outline: none;
}

button:hover {
    color: black;
}

.preview-box pre{
    line-height: 1.2;
}
</style>
