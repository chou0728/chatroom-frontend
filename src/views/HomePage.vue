<template>
  <div class="chat-room">
    <div class="messages">
      <el-scrollbar>
        <div
          v-for="(message, index) in messages"
          :key="index"
          class="message"
          :class="{
            'message-received': message.type === 'received',
            'message-sent': message.type === 'sent'
          }"
        >
          <div class="avatar" :style="{ backgroundImage: 'url(' + message.avatar + ')' }"></div>
          <div class="content">
            <div class="sender-time">{{ message.sender }} · {{ message.time }}</div>
            <div class="text">{{ message.content }}</div>
          </div>
        </div>
      </el-scrollbar>
    </div>
    <div class="input">
      <el-input
        v-model="newMessage"
        placeholder="輸入消息"
        @keypress.enter="sendMessage"
        suffix-icon="el-icon-chat-dot-round"
      />
      <el-button type="primary" @click="sendMessage">發送</el-button>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, reactive } from 'vue'
import type { Ref } from 'vue'
import { ElInput, ElButton, ElScrollbar, ElNotification } from 'element-plus'

interface Message {
  type: string
  avatar: string
  content: string
  sender: string
  time: string
}

const messages: Message[] = reactive([
  {
    type: 'received',
    avatar: 'https://randomuser.me/api/portraits/women/17.jpg',
    content: '你好，有什麼可以幫忙的嗎？',
    sender: '客服小姐姐',
    time: '14:00'
  }
])
const newMessage: Ref<string> = ref('')

const sendMessage = () => {
  if (newMessage.value === '') return

  messages.push({
    type: 'sent',
    avatar: 'https://randomuser.me/api/portraits/men/19.jpg',
    content: newMessage.value,
    sender: '我',
    time: new Date().toLocaleTimeString([], {
      hour: '2-digit',
      minute: '2-digit'
    })
  })
  newMessage.value = ''
  // 隨機模擬客服回應
  setTimeout(() => {
    messages.push({
      type: 'received',
      avatar: 'https://randomuser.me/api/portraits/women/17.jpg',
      content: '您好，我們會盡快為您解決問題。',
      sender: '客服小姐姐',
      time: new Date().toLocaleTimeString([], {
        hour: '2-digit',
        minute: '2-digit'
      })
    })
    ElNotification({
      title: '新消息',
      message: '您有一條新消息',
      type: 'success'
    })
  }, Math.random() * 2000 + 1000)
}
</script>

<style lang="scss" scoped>
.chat-room {
  height: 100%;
  display: flex;
  flex-direction: column;
}

.messages {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
}

.message {
  display: flex;
  margin-bottom: 20px;

  &-received {
    justify-content: flex-start;

    .avatar {
      margin-right: 10px;
    }

    .content {
      background-color: #f2f2f2;
    }
  }

  &-sent {
    justify-content: flex-end;

    .avatar {
      margin-left: 10px;
    }

    .content {
      background-color: #e6f7ff;
    }
  }

  .avatar {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
  }

  .content {
    max-width: 60%;
    padding: 10px;
    border-radius: 10px;

    .sender-time {
      font-size: 12px;
      color: #bfbfbf;
      margin-bottom: 5px;
    }

    .text {
      font-size: 14px;
      word-wrap: break-word;
    }
  }
}

.input {
  display: flex;
  align-items: center;
  padding: 20px;

  .el-input {
    flex: 1;
    margin-right: 20px;
  }
}
</style>
