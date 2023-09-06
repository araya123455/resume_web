<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";
import { useRouter } from 'vue-router'
let routerName = ref(''), _id = ref(''), name = ref(''), price = ref(0);
let router = useRouter()
const API_PATH = import.meta.env.VITE_API_PATH;
onMounted(() => {
  routerName.value = router.currentRoute.value.name;
  _id.value = router.currentRoute.value.params?.id;
  console.log("RouterName", routerName.value);
  console.log("_id", _id.value);
  if (routerName.value == "edit") {
    initEdit();
  }
})
const initEdit = async () => {
  const { data } = await axios.get(`${API_PATH}/getproduct/${_id.value}`);
  name.value = data.data.name
  price.value = data.data.price
  console.log(data);
}
const submit = async () => {
  const req = {
    name: name.value,
    price: price.value,
  };
  console.log(routerName.value);
  if (routerName.value == "create") {
    await axios.post(`${API_PATH}/create`, req
    ).then((response) => {
      name.value = '';
      price.value = '';
      router.push({ name: 'home', replace: true })
      // alert(response.status);
    });
  } else {
    await axios.post(`${API_PATH}/update/${_id.value}`, req
    ).then((response) => {
      router.push({ name: 'home', replace: true })
    })
  }
};
</script>

<template>
  <div class="flex justify-center px-4 py-16">
    <div class="card w-96 bg-base-300 shadow-xl">
      <div class="card-body">
        <div class="form-control w-full max-w-xs">
          <label class="label">
            <span class="label-text-alt">Enter Product Name</span>
          </label>
          <input type="text" placeholder="Enter product name" class="input input-bordered w-full max-w-xs"
            v-model="name" />
          <label class="label">
            <span class="label-text-alt">Enter Product Price</span>
          </label>
          <input type="number" placeholder="Enter product price" class="input input-bordered w-full max-w-xs"
            v-model="price" />
        </div>
        <div class="py-5">
          <button class="btn btn-success" v-on:click="submit()">ADD</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
