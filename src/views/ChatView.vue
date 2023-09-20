<script setup>
import { onMounted, onUpdated, ref, watch } from 'vue';
import {
    app,
    auth,
    database,
    ref as refDb,
    set,
    push,
    onValue
} from '../firebaseConfig'
// import { watch } from 'fs';

let chat = ref("")
const db = refDb(database, 'all_chat/')
let histories = ref([]);
let historykey = ref("")
const bottomEl = ref([])
const studentId = "6314110018"
const onSend = () => {
    if (chat.value != '' && historykey.value != '') {

        push(refDb(database, `all_chat/${historykey.value}`), {
            "user": studentId,
            "message": chat.value,
            "dateTime": new Date().toISOString()
        });
        chat.value = ""
    }
}

onValue(db, (snapshot) => {
    const data = snapshot.val();
    histories.value = data;
})
// watch(histories, (newHistories, oldHistories) => {
//     bottomEl.value.scrollIntoView({ behavior: 'smooth' })
// })
onUpdated(() => {
    bottomEl.value.scrollIntoView({ behavior: 'smooth' })
})
const selectGroup = (key) => {
    historykey.value = key
}
// onMounted(() => {
//     push(refDb(database, 'test'), {
//         "Belly": "Jerramaiar"
//     });
// });

</script>


<template>
    <div class="flex">
    <div class="overflow-y-scroll bg-white h-[90vh] w-[30%]">
        <!-- show test 100 -->
            <!-- <p v-for="i in 100" :key="i">test {{ i }}</p> -->
            <!-- <div class="w-full bg-[#82A189] h-36 border-gray-700 border-4" v-for="i in 100" :key="i">
                                    </div> -->
            <div class="card card-side bg-base-100 shadow-xl w-full mb-4" style="cursor: pointer;"
                v-for="(group, index) in histories" :key="index" data-theme="cupcake" @click="selectGroup(index)">
                <div class="gap-1 card-body">
                    <h2 class="card-title">{{ index }}</h2>
                    <p>{{ group[Object.keys(group)[Object.keys(group).length - 1]].message }}</p>
                </div>
            </div>
        </div>
        <div class="bg-gray-50 h-[90vh] w-[70%]">
            <div class="overflow-y-scroll h-[94%]">
                <!-- <p v-for="i in 100" :key="i">test {{ i }}</p> -->

                <div class="">
                    <!-- <div class="chat-image avatar">
                                                        <div class="w-11 rounded-full">
                                                            <img src="/cat1.jpg" />
                                                        </div>
                                                    </div> -->
                    <!-- <div class="chat-header">
                                                            Kabey
                                                            <time class="text-xs opacity-50">12:45</time>
                                                        </div> -->
                    <!-- <div class="chat-bubble">
                                                    </div> -->
                    <div v-for="(history, index) in histories[historykey]"
                        :class="`chat ${history.user == studentId ? 'chat-end' : 'chat-start'}`" :key="index">
                        <div class="chat-header">
                            {{ history.user }}
                            <time class="text-xs opacity-50">{{ history.dateTime }}</time>
                        </div>
                        <div class="chat-bubble max-w-[45%] break-words">{{ history.message }} </div>
                    </div>
                    <div ref="bottomEl"></div>

                </div>
                <!-- <div class="chat chat-end ">
                                                    <div class="chat-image avatar">
                                                        <div class="w-11 rounded-full">
                                                            <img src="/cat2.jpg" />
                                                        </div>
                                                    </div>
                                                    <div class="chat-header">
                                                        Muffin
                                                        <time class="text-xs opacity-50">12:46</time>
                                                    </div>
                                                    <div class="chat-bubble"></div>
                                                    <div class="chat-footer opacity-50">
                                                        Seen at 12:46
                                                    </div>
                                                </div> -->
            </div>

            <div class="flex h-[6%] gap-1">
                <input v-on:keyup.enter="onSend" v-model="chat" type="text" class="input input-bordered w-[80%] h-full"
                    placeholder="Enter chat" />
                <button @click="onSend" class="w-[20%] btn h-full">send</button>
            </div>
        </div>

    </div>
</template>
<style scoped></style>