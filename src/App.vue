<script setup>
import { ref } from 'vue';
import shortcut from '@/utils/shortcut.js'
import useClipboard from 'vue-clipboard3'

const list = ref(['百度', '必应', 'Google'])
const { toClipboard } = useClipboard()

const clipboardFunc = async (text) => {
  console.log('text: ', text);

  if (text.indexOf('\n') === -1) {
    await toClipboard(text);
    alert('已复制到剪贴板！');
  }
}

shortcut.add("Space", function (e) {
  console.log('Space: ', e);

  clipboardFunc(e.target.innerText)
});

const onClick = (e) => {
  clipboardFunc(e.target.innerText)
}
</script>

<template>
  <div class="w-screen h-screen mx-auto  bg-[#efefe3] px-8">
    <h1 class="text-3xl font-bold underline text-[#376f37] py-4">
      Visual Shortcuts!
    </h1>
    <br>
    <div class="container mx-auto flex">
      <div class="w-3/5 bg-[#d8d8b9] flex flex-col rounded-md p-8 ">
        <a v-for="(item, index) in list" :key="index" href="javascript: void(0)" class="w-full text-base text-[#aab97d] font-medium underline focus:text-lg  hover:text-lg 
          focus:text-[#5a863f] hover:text-[#5a863f]" @click="onClick">
          {{ item }}
        </a><br>
      </div>
      <div class="w-2/5 bg-blue-500">w-3/5</div>

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
