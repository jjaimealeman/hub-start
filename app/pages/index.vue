<template>
  <div class="min-h-screen bg-gray-50">
    <div class="container mx-auto px-4 py-8 max-w-6xl">
      <header class="mb-8">
        <h1 class="text-3xl font-bold text-gray-900 mb-2">
          Hub Start Dashboard
        </h1>
        <p class="text-gray-600">Manage your images, messages, and redirects</p>
      </header>

      <div class="grid gap-8 md:grid-cols-2 xl:grid-cols-3">
        <div class="md:col-span-1">
          <ImageGallery />
        </div>
        <div class="md:col-span-1">
          <MessagesPanel />
        </div>
        <div class="md:col-span-2 xl:col-span-1">
          <RedirectsPanel />
        </div>
      </div>
    </div>
  </div>
</template>
