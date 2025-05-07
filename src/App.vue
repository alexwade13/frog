<template>

    <img :src="selectedImage" alt="Frog" class="frog-image" @error="handleImageError" />
  
</template>

<script setup>
import { ref, onMounted } from 'vue';

const selectedImage = ref('');
const imageList = [];

// Dynamically import all images from the frogs directory
const loadImages = async () => {
  const modules = import.meta.glob('./assets/frogs/*.{jpg,jpeg,png,gif}');
  
  for (const path in modules) {
    try {
      const module = await modules[path]();
      imageList.push(module.default);
    } catch (e) {
      console.error(`Error loading image ${path}:`, e);
    }
  }
  
  selectRandomImage();
};

const selectRandomImage = () => {
  if (imageList.length > 0) {
    selectedImage.value = imageList[Math.floor(Math.random() * imageList.length)];
    console.log('Selected image:', selectedImage.value);
  } else {
    console.warn('No images found in the frogs directory');
  }
};

const handleImageError = () => {
  console.error('Failed to load image:', selectedImage.value);
  selectRandomImage(); // Try another image if one fails to load
};

onMounted(() => {
  loadImages();
});
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow: hidden;
}

.frog-image {
  max-width: 100%;
  max-height: 100vh;
  display: block;
  margin: 0 auto;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
