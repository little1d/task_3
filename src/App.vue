<script setup>
import { marked } from 'marked'
import { ref, onMounted, nextTick } from 'vue'

const showWriteBox = ref(true)
const showPreviewBox = ref(false)
const writeBoxRef = ref(null)
const markdownContent = ref('')

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
            <textarea v-model="markdownContent" ref="writeBoxRef" v-if="showWriteBox" placeholder="Leave a comment" class="google-font write-box"
                cols="90" rows="20"></textarea>
            <div v-if="showPreviewBox" class="preview-box" v-html="marked(markdownContent)"></div>
        </div>

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

.container {
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
}

.write-box:focus {
    border: 3px solid #218bff;
    border-radius: 6px;
}

.write-box:not(:focus) {
    background-color: #f6f8fa;
}

.preview-box {
    width: 500px;
    height: 200px;
    background-color: #fff;
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
    font-family: Roboto, sans-serif;
}

button {
    border: none;
    background-color: transparent;
    outline: none;
}

button:hover {
    color: black;
}</style>
