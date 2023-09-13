<script setup>
import { onMounted, ref } from 'vue';
import {
    app,
    auth,
    database,
    ref as refDb,
    set,
    push
} from '../firebaseConfig'

let chat = ref("")
const studentId = "6314110018"
const onSend = () => {
    push(refDb(database, 'all_chat/group_1/'), {
        "user": studentId,
        "message": chat.value,
        "dataTime": new Date().toISOString()
    });
    chat.value = ""
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
            <div class="w-full bg-[#82A189] h-36 border-gray-700 border-4" v-for="i in 100" :key="i"></div>
        </div>
        <div class="bg-gray-50 h-[90vh] w-[70%]">
            <div class="overflow-y-scroll h-[94%]">
                <!-- <p v-for="i in 100" :key="i">test {{ i }}</p> -->
                <div class="chat chat-start ">
                    <div class="chat-image avatar">
                        <div class="w-11 rounded-full">
                            <img src="/cat1.jpg" />
                        </div>
                    </div>
                    <div class="chat-header">
                        Kabey
                        <time class="text-xs opacity-50">12:45</time>
                    </div>
                    <div class="chat-bubble">Hey where my food!</div>
                    <div class="chat-footer opacity-50">
                        Delivered
                    </div>
                </div>
                <div class="chat chat-end ">
                    <div class="chat-image avatar">
                        <div class="w-11 rounded-full">
                            <img src="/cat2.jpg" />
                        </div>
                    </div>
                    <div class="chat-header">
                        Muffin
                        <time class="text-xs opacity-50">12:46</time>
                    </div>
                    <div class="chat-bubble">I hate you!</div>
                    <div class="chat-footer opacity-50">
                        Seen at 12:46
                    </div>
                </div>
            </div>
            <div class="flex h-[6%] gap-1">
                <input v-on:keyup.enter="onSend" v-model="chat" type="text" class="input input-bordered w-[80%] h-full" placeholder="Enter chat" />
                <button class="w-[20%] btn h-full" @click="onSend">send</button>
            </div>
        </div>

    </div>
</template>
<style scoped></style>