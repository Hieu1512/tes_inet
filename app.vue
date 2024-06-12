// import { ref } from 'vue' // const message = ref('Hello World!') //
console.log(message.value) // "Hello World!" // message.value = 'Changed'

<script setup>
// watch là một hàm dùng để theo dõi các thay đổi của một biến.
import { reactive, ref, computed, onMounted, watch } from "vue";

import nuxt from "./header.vue";

// import JSConfetti from 'js-confetti'

let id = 0;

const sodem = reactive({ count: 22 });
const tinnhan = ref("So hien thi o duoi");
const baidau = ref("doimau");
const sobam = ref("0");
const chu = ref("");
const bamdehien = ref("");
const doiicon = ref(true);

// Biến để lưu trữ nội dung của todo mới
const todomoi = ref("");
const antodo = ref(false);
// Mảng chứa danh sách các todo
const todos = ref([
  { id: id++, text: "VietNam", done: true },
  { id: id++, text: "HaNoi", done: false },
]);

const loctodo = computed(() => {
  return antodo.value ? todos.value.filter((t) => !t.done) : todos.value;
});
const koco = ref(null);
onMounted(() => {
  koco.value.textContent = " cha hieu kieu gi";
});

const todoId = ref(1);
const todoData = ref(null);

const vietra = ref("xin chao");

const childMsg = ref("chua co tin nhan con");

const msg = ref("from parent");

// const confetti = new JSConfetti()

function bamtang() {
  sobam.value++;
}
function daura(e) {
  chu.value = e.target.value;
}
function thaydoi() {
  doiicon.value = !doiicon.value;
}
// Hàm để thêm một todo mới vào danh sách
// một đối tượng mới được tạo với id tăng dần và text được lấy từ todomoi.value. Đối tượng này sau đó được thêm vào mảng todos thông qua todos.value.push().
// Hàm để xoa một todo trong danh sách
function them() {
  todos.value.push({ id: id++, text: todomoi.value, done: false });
  todomoi.value = "";
}
function xoa(todo) {
  todos.value = todos.value.filter((t) => t !== todo);
}
async function timdata() {
  todoData.value = null;
  const res = await fetch(`https://jsonplaceholder.typicode.com/todos/${todoId.value}`);
}

//  function showConfetti() {
//   confetti.addConfetti()
// }

// showConfetti()
</script>

<template>
  <h1>{{ tinnhan }}</h1>

  <p>so la : {{ sodem.count }}</p>

  <button @click="bamtang">Bam de tang : {{ sobam }}</button>
  <br />
  <input :value="chu" @input="daura" placeholder="go de hien ra ben duoi " />
  <br />
  <p>{{ chu }}</p>
  <input v-model="bamdehien" placeholder="go va enter de hien ra ben duoi" />
  <p>{{ bamdehien }}</p>

  <button @click="thaydoi">thay doi</button>
  <h2 v-if="doiicon">vvvvvv</h2>
  <h3 v-else>kkkkkk</h3>

  <div>
    <h1 :class="baidau">Bai dau</h1>
    <!-- Form để người dùng nhập todo mới -->
    <form @submit.prevent="them">
      <!-- @submit.prevent được sử dụng để ngăn chặn hành vi mặc định của biểu mẫu, tức là không gửi yêu cầu lại server và làm tải lại trang. -->
      <input v-model="todomoi" required placeholder="them todo moi tai day" />
      <button type="submit">Them Todo</button>
    </form>

    <!-- Danh sách các todo hiện có -->
    <ul>
      <li v-for="todo in loctodo" :key="todo.id">
        <input type="checkbox" v-model="todo.done" />
        <span :class="{ done: todo.done }"> {{ todo.text }}</span>
        <button @click="xoa(todo)">X</button>
      </li>
    </ul>
    <button @click="antodo = !antodo">
      {{ antodo ? "Hien tat" : "An di " }}
    </button>
  </div>
  <p ref="koco"></p>

  <!-- <nuxt :msg="vietra"/> -->
  <nuxt @response="(msg) => (childMsg = msg)" />
  <p>{{ childMsg }}</p>
  <nuxt>Message: {{ msg }}</nuxt>
  <!-- <h1 @click="showConfetti" class="doimausac">🎉 Congratulations!</h1> -->
</template>

<style>
.doimau {
  color: aqua;
}
.done {
  text-decoration: line-through;
}
.doimausac {
  text-align: center;
  cursor: pointer;
  margin-top: 3em;
  color: brown;
}
</style>
