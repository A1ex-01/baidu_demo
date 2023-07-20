<script setup lang="ts">
import { ref } from "vue";
import lodash from "lodash-es";
import data from "./data.json";
interface IRes {
  id: number;
  text: string;
}
const dropDownState = ref(false);
async function getData(v: string): Promise<IRes[]> {
  console.log("getData...");
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(data.filter((item) => item.text.startsWith(v)));
    }, 100);
  });
}
const inputRef = ref<HTMLInputElement>();
const list = ref<IRes[]>([]);

const searchData = lodash.debounce(async (e: any) => {
  list.value = await getData(e.target?.value);
  dropDownState.value = true;
}, 300);

const itemClick = (e: any) => {
  inputRef.value!.value = e.target?.innerText;
  searchData(inputRef.value);
  inputRef.value?.focus();
};

const resetData = () => {
  setTimeout(() => {
    dropDownState.value = false;
  }, 200);
};

const clear = () => {
  dropDownState.value = false;
  inputRef.value!.value = "";
  list.value = [];
};
</script>

<template>
  <div>
    <div class="logo">
      <img
        src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png"
        width="270"
        height="129"
        alt="百度"
      />
    </div>
    <div class="search">
      <input
        type="text"
        ref="inputRef"
        @input="searchData"
        @focus="searchData"
        @blur="resetData"
      />
      <button>百度一下</button>
      <svg
        v-if="inputRef?.value"
        t="1689824231384"
        class="icon"
        viewBox="0 0 1024 1024"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        p-id="3131"
        width="200"
        height="200"
        @click="clear"
      >
        <path
          d="M504.224 470.288l207.84-207.84a16 16 0 0 1 22.608 0l11.328 11.328a16 16 0 0 1 0 22.624l-207.84 207.824 207.84 207.84a16 16 0 0 1 0 22.608l-11.328 11.328a16 16 0 0 1-22.624 0l-207.824-207.84-207.84 207.84a16 16 0 0 1-22.608 0l-11.328-11.328a16 16 0 0 1 0-22.624l207.84-207.824-207.84-207.84a16 16 0 0 1 0-22.608l11.328-11.328a16 16 0 0 1 22.624 0l207.824 207.84z"
          fill="#999"
          p-id="3132"
        ></path>
      </svg>
      <ul class="drop-down" @click="itemClick" v-if="dropDownState">
        <li v-for="item in list" :key="item.id">
          {{ item.text }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.search {
  width: 654px;
  height: 44px;
  display: flex;
  border-radius: 10px;
  border: 2px solid #c4c7ce;
  box-sizing: border-box;
  position: relative;
  z-index: 999;
  > input {
    flex: 1;
    border: 0;
    border-radius: 10px;
    padding-left: 10px;
    font-size: 16px;
    &:focus {
      outline: none;
    }
  }

  > button {
    width: 108px;
    background-color: #4e6ef2;
    cursor: pointer;
    border-radius: 0;
    border-top-right-radius: 10px;
    border-bottom-right-radius: 10px;
    color: white;
  }
  .drop-down {
    position: absolute;
    width: 546px;
    border: 2px solid #4e6ef2;
    border-top: 0px;
    text-align: left;
    background-color: white;
    padding: 0;
    left: -2px;
    bottom: 8px;
    transform: translateY(100%);
    margin: 0;
    color: #666;
    list-style-type: none;
    box-sizing: border-box;
    font-size: 14px;
    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;
    overflow: hidden;
    > li {
      padding: 10px 10px;
      box-sizing: border-box;
      cursor: pointer;
      &:hover {
        background-color: #f5f6f9;
        color: #317ffa;
      }
    }
    &:empty {
      border: 0;
    }
  }
  .icon {
    position: absolute;
    right: 120px;
    top: 0;
    width: 24px;
    height: 40px;
    cursor: pointer;
  }
}

.search:focus-within {
  border: 2px solid #4e6ef2;
}
</style>
