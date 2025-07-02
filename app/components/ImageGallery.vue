<script setup lang="ts">
const { data: images, refresh } = await useFetch("/api/images");

async function uploadImage(e: Event) {
  // https://hub.nuxt.com/docs/storage/blob#useupload
  const upload = useUpload("/api/images/upload", {
    multiple: false,
  });
  const form = e.target as HTMLFormElement;
  await upload(form.image)
    .then(async () => {
      form.reset();
      await refresh();
    })
    .catch((err) => alert("Failed to upload image:\n" + err.data?.message));
}

async function deleteImage(pathname: string) {
  await $fetch(`/api/images/${pathname}`, { method: "DELETE" });
  await refresh();
}
</script>

<template>
  <div class="bg-white rounded-lg shadow-sm border border-gray-200 p-6 h-fit">
    <div class="flex items-center gap-2 mb-6">
      <div
        class="w-8 h-8 bg-blue-100 rounded-lg flex items-center justify-center"
      >
        <UIcon name="i-tabler-photo" class="size-5 text-blue-500" />
      </div>
      <h3 class="text-lg font-semibold text-gray-900">Images</h3>
    </div>

    <form class="mb-6" @submit.prevent="uploadImage">
      <div class="space-y-3">
        <label class="block text-sm font-medium text-gray-700">
          Upload an image
        </label>
        <div class="flex items-center gap-3">
          <input
            type="file"
            name="image"
            accept="image/jpg,image/png"
            class="block w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-lg file:border-0 file:text-sm file:font-medium file:bg-blue-50 file:text-blue-700 hover:file:bg-blue-100 file:cursor-pointer"
          />
          <button
            type="submit"
            class="px-4 py-2 bg-blue-600 text-white text-sm font-medium rounded-lg hover:bg-blue-700 focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition-colors"
          >
            Upload
          </button>
        </div>
      </div>
    </form>

    <div v-if="images?.length" class="space-y-4">
      <div class="grid grid-cols-2 gap-3">
        <img
          v-for="image of images"
          :key="image.pathname"
          width="200"
          :src="`/images/${image.pathname}`"
          :alt="image.pathname"
          @dblclick="deleteImage(image.pathname)"
        />
      </div>
      <p class="text-xs text-gray-500 text-center">
        💡 Tip: Double-click on an image to delete it
      </p>
    </div>

    <div v-else class="text-center py-8">
      <UIcon name="i-tabler-photo" class="size-12 text-gray-300" />
      <p class="text-gray-500 text-sm">No images uploaded yet</p>
    </div>
  </div>
</template>
