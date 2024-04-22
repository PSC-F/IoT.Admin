<script setup lang="ts">
import { ref, onMounted } from "vue";
import * as signalR from "@microsoft/signalr";

// SignalR 服务端的 URL
const serverUrl = "http://localhost:5231/DataHub";
const connected = ref(false);
const receivedMessage = ref("");

// 创建 SignalR 连接
const connection = new signalR.HubConnectionBuilder()
  .withUrl(serverUrl)
  .build();

onMounted(async () => {
  try {
    // 连接到 SignalR 服务端
    await connection.start();
    connected.value = true;
    console.log("Connected to SignalR server");

    // 注册接收消息的处理程序
    connection.on("ReceiveMessage", (topic, message) => {
      receivedMessage.value = `${topic}, ${message}`;
      console.log(`Received message: ${topic},${message}`);
    });
  } catch (error) {
    console.error("Failed to connect to SignalR server:", error);
  }
});
</script>

<template>
  <div>
    <h1>实时数据测试</h1>
    <h2>{{ receivedMessage }}</h2>
    <h2>状态{{ connected }}</h2>
  </div>
</template>
