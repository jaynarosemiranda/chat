<template>
    <div>
        <div class="chat-container">
            <div class="px-2 py-3 chats" style="width: 18%">
                <div class="flex items-center justify-between">
                    <span
                        class="bg-slate-300 w-7 h-7 rounded-full inline-block flex justify-center items-center cursor-pointer"
                    >
                        <font-awesome-icon
                            :icon="['far', 'fa-user']"
                            class="text-sm text-indigo-400"
                        />
                    </span>

                    <div
                        class="rounded-xl flex justify-between items-center w-52 px-2 py-1 search-container"
                    >
                        <font-awesome-icon
                            :icon="['fas', 'fa-magnifying-glass']"
                            class="text-sm text-indigo-400 pr-2"
                        />
                        <input
                            type="search"
                            placeholder="Search..."
                            class="border-none text-sm outline-none placeholder:text-xs text-white"
                        />
                    </div>

                    <span
                        class="bg-slate-300 w-7 h-7 rounded-full inline-block flex justify-center items-center cursor-pointer"
                    >
                        <font-awesome-icon
                            :icon="['far', 'fa-pen-to-square']"
                            class="text-sm text-indigo-400"
                        />
                    </span>
                </div>

                <!-- Chats -->

                <p class="mt-4 text-white text-sm">Chats</p>
                <div class="flex">
                    <span
                        class="plus-container w-6 h-6 rounded-md inline-block flex justify-center items-center cursor-pointer my-2 mr-2"
                    >
                        <font-awesome-icon
                            :icon="['fas', 'fa-list']"
                            class="text-sm text-indigo-400"
                        />
                    </span>
                    <span
                        class="plus-container w-6 h-6 rounded-md inline-block flex justify-center items-center cursor-pointer my-2 mr-2"
                    >
                        <font-awesome-icon
                            :icon="['fas', 'fa-plus']"
                            class="text-sm text-indigo-400"
                        />
                    </span>
                </div>

                <div class="text-white">
                    <ul class="">
                        <li
                            class="text-xs mt-2 flex p-2 chat-box-left rounded-lg mt-1 w-full flex-col"
                            v-for="(m, index) in messages.slice(0, 1)"
                            :key="index"
                        >
                            <div class="text-white"></div>

                            <div class="flex justify-between">
                                <div class="flex">
                                    <img
                                        src="@/assets/images/logo.jpeg"
                                        class="w-8 h-8 rounded-full mr-2"
                                    />
                                    <div class="flex flex-col">
                                        <a
                                            href=""
                                            class="text-sm font-semi-bold"
                                            >Dev team</a
                                        >
                                        <div
                                            v-if="m.me === false"
                                            class="line-clamp-2"
                                        >
                                            {{ m.message }}
                                        </div>
                                    </div>
                                </div>

                                <span
                                    class="chat-box-left-time font-bold text-end"
                                    >5:00 PM</span
                                >
                            </div>
                        </li>
                    </ul>
                </div>
            </div>

            <div class="" style="width: 82%">
                <div class="text-white flex p-3 items-center chat-message p-2">
                    <img
                        src="@/assets/images/logo.jpeg"
                        class="w-10 h-10 rounded-full"
                    />
                    <div>
                        <h1 class="pl-3">Dev Team</h1>
                        <a href="" class="text-indigo-400 text-xs pl-3 hover"
                            >8 Participants</a
                        >
                    </div>
                </div>

                <div
                    class="flex flex-col w-full p-12 items-center conversation-container"
                >
                    <ul class="flex justify-between flex-col text-white">
                        <li v-for="(m, index) in messages" :key="index">
                            <div class="flex justify-between">
                                <div
                                    v-if="m.me === true"
                                    class="flex items-center p-2 chat-box-reply rounded-l-xl rounded-b-xl mt-3 w-80 ml-96"
                                    style="word-wrap: break-word"
                                >
                                    {{ m.message }}
                                </div>
                                <div
                                    v-if="m.me === false"
                                    class="flex items-center p-2 chat-box rounded-r-xl rounded-b-xl mt-3 w-80 mr-96"
                                    style="word-wrap: break-word"
                                >
                                    {{ m.message }}
                                </div>
                            </div>
                        </li>
                    </ul>
                </div>

                <div
                    class="flex input-message p-2 bottom-0 absolute"
                    style="width: 82%"
                >
                    <a href="">
                        <font-awesome-icon
                            :icon="['fas', 'fa-plus']"
                            class="text-sm pr-2 text-indigo-400"
                        />
                    </a>

                    <input
                        type="text"
                        class="w-full outline-none text-white text-sm placeholder:text-xs placeholder:italic"
                        placeholder="Type a message..."
                        id="chatInput"
                        @keypress="chatInput"
                    />
                    <button
                        class="text-indigo-400 text-sm hover:text-indigo-500"
                    >
                        Send
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import io from "socket.io-client";
export default {
    data() {
        return {
            socket: null,
            messages: [],
        };
    },
    methods: {
        chatInput(e) {
            let vm = this;
            // console.log(e);
            if (e.keyCode === 13 && !e.shiftKey) {
                vm.socket.emit("sendChatToServer", e.target.value);
                vm.messages.push({ me: true, message: e.target.value });
                // vm.messages.push(e.target.value);
                e.target.value = "";
                return false;
            }
        },
    },
    mounted() {
        let vm = this;
        vm.socket = io("178.128.221.73:3000");
        vm.socket.on("sendChatToClient", (message) => {
            // console.log(message);
            vm.messages.push({ me: false, message: message });
        });
    },
};
</script>

<style scoped>
.chat-container {
    background: #1e2733;
    width: 100%;
    height: 100vh;
    overflow-x: hidden;
    display: flex;
}
.fa-magnifying-glass {
    color: rgb(229, 231, 235);
}
.chat-box {
    background: #3f4c5c;
}

.chat-box-left {
    background: #243448;
}
.chat-box-left-time {
    font-size: 10px;
    color: #3f4c5c;
}
.chats {
    border-right: 1px solid #354657;
}
.search-container {
    border: none;
    background: #3f4c5c;
}

input {
    background: inherit;
}
.chat-message {
    border-bottom: 1px solid #354657;
}
.input-message {
    border-top: 1px solid #354657;
}
.conversation-container {
    height: 88%;
}
.hover:hover {
    text-decoration: underline;
}
.chat-box-reply {
    background: #163b60;
}

.plus-container {
    background: #354657;
}
.plus-container:hover {
    background: #475d74;
}
.right {
    background: red;
}
</style>
