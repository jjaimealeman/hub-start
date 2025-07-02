<script setup lang="ts">
const { data: redirects, refresh } = await useFetch("/api/redirects", {
  transform: (data: { [key: string]: string }) => {
    // Transform to text for the textarea
    return {
      text: Object.entries(data)
        .map(([from, to]) => `${from} ${to}`)
        .join("\n"),
    };
  },
});

async function updateRedirects() {
  const body = Object.fromEntries(
    redirects.value!.text.split("\n").map((line) => line.split(" "))
  );
  await $fetch("/api/redirects", {
    method: "PUT",
    body,
  });
  await refresh();
}
</script>

<template>
  <div class="bg-white rounded-lg shadow-sm border border-gray-200 p-6 h-fit">
    <div class="flex items-center gap-2 mb-6">
      <div
        class="w-8 h-8 bg-purple-100 rounded-lg flex items-center justify-center"
      >
        <UIcon
          name="i-tabler-arrow-forward-up"
          class="size-5 text-purple-500"
        />
      </div>
      <h3 class="text-lg font-semibold text-gray-900">Server Redirects</h3>
    </div>

    <form class="space-y-4" @submit.prevent="updateRedirects">
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-2">
          Redirect Rules
        </label>
        <textarea
          v-model="(redirects || { text: '' }).text"
          rows="8"
          placeholder="/from /to&#10;/old-path /new-path&#10;/about /about-us"
          class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-purple-500 outline-none text-sm font-mono resize-none placeholder:text-gray-400 text-purple-500"
        />
        <p class="text-xs text-gray-500 mt-2">
          💡 One redirect per line in the format:
          <code class="bg-gray-100 px-1 rounded">/from /to</code>
        </p>
      </div>

      <button
        type="submit"
        class="w-full px-4 py-2 bg-purple-600 text-white text-sm font-medium rounded-lg hover:bg-purple-700 focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 transition-colors"
      >
        <div class="flex items-center justify-center gap-2">
          <UIcon name="i-tabler-check" class="size-5 text-purple-100" />
          Save Redirects
        </div>
      </button>
    </form>

    <div class="mt-6 p-4 bg-gray-50 rounded-lg">
      <h4 class="text-sm font-medium text-gray-900 mb-2">How it works</h4>
      <ul class="text-xs text-gray-600 space-y-1">
        <li>• Each line represents one redirect rule</li>
        <li>
          • Format:
          <code class="bg-white px-1 rounded"
            >/source-path /destination-path</code
          >
        </li>
        <li>• Server will automatically redirect visitors</li>
        <li>• Changes take effect immediately after saving</li>
      </ul>
    </div>
  </div>
</template>
