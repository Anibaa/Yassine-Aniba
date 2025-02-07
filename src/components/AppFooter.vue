<template>
  <v-footer class="text-center d-flex flex-column pa-4">
    <div class="social-icons mb-4">
      <v-btn
        v-for="icon in socialIcons"
        :key="icon.icon"
        :href="icon.link"
        icon
        variant="text"
        target="_blank"
        class="mx-2 social-icon"
      >
        <v-icon>{{ icon.icon }}</v-icon>
      </v-btn>
    </div>
    <div class="text-body-2 text-medium-emphasis">
      © {{ new Date().getFullYear() }} - Yassine Aniba. All rights reserved.
    </div>
  </v-footer>
</template>

<script setup>
const socialIcons = [
  { icon: 'mdi-github', link: 'https://github.com/Anibaa' },
  { icon: 'mdi-linkedin', link: 'https://www.linkedin.com/in/yassine-aniba-63a981224/' },
]
</script>

<style scoped>
.social-icon {
  transition: transform 0.3s ease;
}

.social-icon:hover {
  transform: translateY(-3px);
}

.v-footer {
  position: relative;
  bottom: 0;
  width: 100%;
}
</style> 