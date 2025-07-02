<script setup lang="ts">
const { data: messages, refresh } = await useFetch("/api/messages");
const newMessage = ref("");

async function sendMessage() {
  if (!newMessage.value.trim()) return;
  await $fetch("/api/messages", {
    method: "POST",
    body: {
      text: newMessage.value,
    },
  });
  newMessage.value = "";
  await refresh();
}
</script>

<template>
  <div
    class="bg-white rounded-lg shadow-sm border border-gray-200 flex flex-col h-96"
  >
    <div class="flex items-center gap-2 p-6 pb-4 border-b border-gray-100">
      <div
        class="w-8 h-8 bg-green-100 rounded-lg flex items-center justify-center"
      >
        <UIcon name="i-tabler-message" class="size-5 text-green-500" />
      </div>
      <h3 class="text-lg font-semibold text-gray-900">Messages</h3>
    </div>

    <div class="flex-1 overflow-y-auto p-4 space-y-3">
      <div v-if="messages?.length" class="space-y-3">
        <div
          v-for="message of messages"
          :key="message.id"
          class="bg-gray-50 rounded-lg p-3 border-l-4 border-blue-500"
        >
          <p class="text-gray-900 text-sm">{{ message.text }}</p>
          <p class="text-xs text-gray-500 mt-1">
            {{
              message.created_at
                ? new Date(message.created_at as string).toLocaleString("fr-FR")
                : "No date"
            }}
          </p>
        </div>
      </div>

      <div
        v-else
        class="flex-1 flex items-center justify-center text-center py-8"
      >
        <div>
          <UIcon name="i-tabler-message" class="size-12 text-gray-300" />

          <p class="text-gray-500 text-sm">No messages yet</p>
          <p class="text-gray-400 text-xs mt-1">
            Send your first message below
          </p>
        </div>
      </div>
    </div>

    <div class="p-4 border-t border-gray-100">
      <form class="flex gap-2" @submit.prevent="sendMessage">
        <input
          v-model="newMessage"
          placeholder="Type a message..."
          class="flex-1 px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none text-sm placeholder:text-gray-400 text-blue-500"
        />
        <button
          type="submit"
          :disabled="!newMessage.trim()"
          class="px-4 py-2 bg-blue-600 text-white text-sm font-medium rounded-lg hover:bg-blue-700 focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 disabled:opacity-50 disabled:cursor-not-allowed transition-colors"
        >
          <UIcon name="i-tabler-send" class="size-5 text-blue-100" />
        </button>
      </form>
    </div>
  </div>
</template>
