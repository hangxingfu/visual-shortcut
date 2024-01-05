<script setup>
import { ref, watchEffect } from 'vue';
import shortcut from '@/utils/shortcut.js'
import useClipboard from 'vue-clipboard3'

const list = ref([])
const listPage = ref([])
const { toClipboard } = useClipboard()
const titleCase = (str) => {
  return str.toLowerCase().replace(/( |^)[a-z]/g, (L) => L.toUpperCase());
}

watchEffect(() => {
  let arr = []
  list.value.forEach(item => {

    switch (item) {
      case 'Meta':
        item = 'Command'
        break;
      case ' ':
        item = 'Space'
        break;
    }
    arr.push(titleCase(item))
  })

  listPage.value.push(arr.join(' + '))
})



const clipboardFunc = async (text) => {
  if (text.indexOf('\n') === -1) {
    await toClipboard(text);
    alert('已复制到剪贴板！');
  }
}

shortcut.add("Space", function (e) {
  clipboardFunc(e.target.innerText)
});

const onClick = (e) => {
  clipboardFunc(e.target.innerText)
}

// =========================================
// 监听
// 普通两键示例
let firstTime = 0
let doubleEvent = {}
document.addEventListener('keydown', (e) => {
  let currentTime = Date.now()
  if (firstTime === 0) {
    firstTime = currentTime
    doubleEvent[e.key] = true
  } else if (currentTime - firstTime > 1000) {//超过300ms
    firstTime = 0
    doubleEvent = {}
  } else {
    doubleEvent[e.key] = true
  }
})

document.addEventListener('keyup', (e) => {
  if (Object.keys(doubleEvent).length > 1) {
    // Object.keys(doubleEvent).forEach(i => {
    // list.value.push(i);
    list.value = Object.keys(doubleEvent);
    // })
    doubleEvent = {}
    firstTime = 0
  }
})
</script>

<template>
  <div class="w-screen min-h-screen mx-auto  bg-[#efefe3] px-8">
    <h1 class="text-3xl font-bold underline text-[#376f37] py-4">
      Visual Shortcuts!
    </h1>
    <br>
    <div class="container mx-auto flex">
      <div class="w-2/5 ">
        <article class="text-wrap text-[#47310f]">
          <h2>自动监听您按下的键盘键。</h2>
          <p>Tab / Shift + Tab 键快速选取，空格键复制当前文本。</p>
          <p> 您也可以用鼠标点击想要复制的快捷键文本。</p>
        </article>
      </div>
      <div class="w-3/5 bg-[#d8d8b9] flex flex-col rounded-md p-8">
        <a v-for="(item, index) in listPage" :key="index" href="javascript: void(0)" class="text-base text-[#aab97d] font-medium underline focus:text-lg  hover:text-lg 
          focus:text-[#5a863f] hover:text-[#5a863f]" @click="onClick">
          {{ item }}
        </a>
      </div>

      <!-- <div class="w-1/2 ">
        <a v-for="(item, index) in list" :key="index" href="javascript: void(0)" class="text-base text-[#aab97d] font-medium underline focus:text-lg  hover:text-lg 
          focus:text-[#5a863f] hover:text-[#5a863f]" @click="onClick">
          {{ item }}
        </a><br>
      </div>
      <div class="w-1/2">
        <div class="absolute inset-y-0 right-0 w-16">06</div>
      </div> -->
    </div>
  </div>
</template>

<style scoped></style>
