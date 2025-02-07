<template>
  <v-app-bar 
    app 
    :elevation="scrolled ? 1 : 0" 
    :class="{ 
      'bg-transparent': !scrolled,
      'header-scrolled': scrolled 
    }"
  >
    <v-container>
      <!-- Logo -->
      <div class="d-flex align-center justify-space-between w-100">
        <v-app-bar-title>
          <div class="logo-wrapper">
            <span class="gradient-text font-weight-bold">Yassine Aniba</span>
            <div class="logo-glow"></div>
          </div>
        </v-app-bar-title>

        <!-- Desktop Navigation -->
        <div class="d-none d-md-flex align-center">
          <div class="nav-links">
            <v-btn
              v-for="item in menuItems"
              :key="item.title"
              :href="item.href"
              variant="text"
              class="nav-link text-none"
              :class="{ active: activeSection === item.href }"
              @click="setActiveSection(item.href)"
            >
              {{ item.title }}
            </v-btn>
          </div>
          
          <!-- <v-btn
            class="ml-4 contact-btn"
            color="primary"
            elevation="0"
            href="#contact"
          >
            Let's Talk
            <v-icon right class="ml-2">mdi-arrow-right</v-icon>
          </v-btn> -->

          <v-btn
            class="ml-4 download-btn white--text"
            color="secondary"
            elevation="0"
            href="/YassineAniba_Resume.pdf"
            download
          >
            Download CV
            <v-icon right class="ml-2">mdi-download</v-icon>
          </v-btn>
        </div>

        <!-- Mobile Menu Button -->
        <v-app-bar-nav-icon
          class="d-flex d-md-none"
          @click="drawer = !drawer"
        ></v-app-bar-nav-icon>
      </div>
    </v-container>
  </v-app-bar>

  <!-- Mobile Navigation Drawer -->
  <v-navigation-drawer
    v-model="drawer"
    location="right"
    temporary
  >
    <v-list>
      <v-list-item
        v-for="item in menuItems"
        :key="item.title"
        :href="item.href"
        @click="setActiveSection(item.href); drawer = false"
      >
        <v-list-item-title>{{ item.title }}</v-list-item-title>
      </v-list-item>
      
      <v-divider class="my-2"></v-divider>
      
      <!-- <v-list-item>
        <v-btn
          block
          color="primary"
          elevation="0"
          href="#contact"
        >
          Let's Talk
          <v-icon right class="ml-2">mdi-arrow-right</v-icon>
        </v-btn>
      </v-list-item> -->

      <v-list-item>
        <v-btn
          block
          class="download-btn white--text"
          color="secondary"
          elevation="0"
          href="/YassineAniba_Resume.pdf"
          download
        >
          Download CV
          <v-icon right class="ml-2">mdi-download</v-icon>
        </v-btn>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const scrolled = ref(false)
const activeSection = ref('#home')
const drawer = ref(false)

const menuItems = [
  { title: 'Home', href: '#home' },
  { title: 'About', href: '#about' },
  { title: 'Projects', href: '#projects' },
  { title: 'Contact', href: '#contact' },
]

// Enhanced scroll handling with section detection
const handleScroll = () => {
  scrolled.value = window.scrollY > 20
  
  // Get all sections
  const sections = menuItems.map(item => ({
    id: item.href.substring(1),
    element: document.getElementById(item.href.substring(1))
  })).filter(section => section.element)

  // Find the current section
  const currentSection = sections.find(section => {
    const element = section.element
    const rect = element.getBoundingClientRect()
    // Consider a section "active" when it's in the middle third of the viewport
    const viewportHeight = window.innerHeight
    const triggerPoint = viewportHeight / 3
    
    return rect.top <= triggerPoint && rect.bottom >= triggerPoint
  })

  // Update active section
  if (currentSection) {
    activeSection.value = `#${currentSection.id}`
  }
}

// Smooth scroll to section
const setActiveSection = (section) => {
  activeSection.value = section
  const element = document.querySelector(section)
  if (element) {
    element.scrollIntoView({
      behavior: 'smooth',
      block: 'start'
    })
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  // Initial check for active section
  handleScroll()
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
.v-app-bar {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.bg-transparent {
  background: transparent !important;
  backdrop-filter: blur(10px);
}

.logo-wrapper {
  position: relative;
  padding: 8px;
}

.logo-glow {
  position: absolute;
  top: 50%;
  left: -100%;
  width: 50px;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(64, 223, 159, 0.3),
    rgba(59, 130, 246, 0.3),
    transparent
  );
  transform: translateY(-50%) skewX(-20deg);
  animation: logoGlow 3s infinite;
}

.gradient-text {
  background: linear-gradient(45deg, #40DF9F, #3B82F6);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  font-size: 1.75rem;
  font-weight: 700;
  letter-spacing: -0.5px;
}

.nav-link {
  position: relative;
  margin: 0 4px;
  font-weight: 500;
  transition: all 0.3s ease;
  padding: 6px 16px;
  border-radius: 8px;
}

.nav-link.active {
  background: linear-gradient(
    90deg,
    rgba(64, 223, 159, 0.15),
    rgba(59, 130, 246, 0.15)
  );
  color: #40DF9F;
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: 2px;
  left: 50%;
  width: 0;
  height: 3px;
  background: linear-gradient(90deg, #40DF9F, #3B82F6);
  transition: all 0.3s ease;
  transform: translateX(-50%);
  border-radius: 4px;
}

.nav-link:hover::after,
.nav-link.active::after {
  width: 30px;
}

.contact-btn {
  background: linear-gradient(45deg, #40DF9F, #3B82F6) !important;
  border-radius: 8px;
  transition: all 0.3s ease !important;
}

.contact-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(64, 223, 159, 0.3);
}

.header-scrolled {
  background: rgba(75, 75, 75, 0.623) !important;
  backdrop-filter: blur(15px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
}

/* Mobile Styles */
@media (max-width: 959px) {
  .v-container {
    padding: 8px 16px;
  }
  
  .gradient-text {
    font-size: 1.5rem;
  }

  .v-list-item {
    padding: 12px 16px;
  }

  .v-btn.contact-btn {
    margin-top: 16px;
    width: 100%;
    height: 48px;
    font-size: 1rem;
  }

  .v-btn.download-btn {
    margin-top: 16px;
    width: 100%;
    height: 48px;
    font-size: 1rem;
  }
}

/* Tablet Styles */
@media (min-width: 600px) and (max-width: 959px) {
  .nav-link {
    padding: 0 12px;
  }
}

/* Navigation Drawer Styles */
.v-navigation-drawer {
  padding-top: 20px;
  background: rgba(75, 75, 75, 0.623) !important;
  backdrop-filter: blur(15px);
}

.v-list {
  background: transparent;
  padding: 16px;
}

.v-list-item {
  margin: 8px 0;
  border-radius: 8px;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.v-list-item-title {
  font-weight: 500;
  letter-spacing: 0.5px;
}

.v-list-item.active {
  background: linear-gradient(
    90deg,
    rgba(64, 223, 159, 0.2),
    rgba(59, 130, 246, 0.2)
  );
  color: #40DF9F;
}

.v-list-item.active::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  height: 100%;
  width: 4px;
  background: linear-gradient(45deg, #40DF9F, #3B82F6);
  border-radius: 0 4px 4px 0;
}

.v-list-item:hover {
  background: rgba(64, 223, 159, 0.15);
  transform: translateX(4px);
}

.header-scrolled .nav-link.active {
  background: linear-gradient(
    90deg,
    rgba(64, 223, 159, 0.2),
    rgba(59, 130, 246, 0.2)
  );
  box-shadow: 0 2px 10px rgba(64, 223, 159, 0.1);
}

/* Enhanced Mobile Active States */
@media (max-width: 959px) {
  .v-list-item.active {
    padding-left: 20px;
  }

  .v-list-item.active .v-list-item-title {
    font-weight: 600;
    background: linear-gradient(45deg, #40DF9F, #3B82F6);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
  }
  
  .v-list-item::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 16px;
    right: 16px;
    height: 1px;
    background: linear-gradient(
      90deg,
      transparent,
      rgba(255, 255, 255, 0.1),
      transparent
    );
  }
}

/* Enhanced Hover Effects */
.nav-link:hover {
  background: rgba(64, 223, 159, 0.1);
  transform: translateY(-1px);
}

.nav-links {
  display: flex;
  gap: 8px;
}

/* Pulse Animation for Active Item */
@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}

.nav-link.active {
  animation: pulse 2s infinite;
}

/* Enhanced Active Section Indicator */
.nav-link.active {
  background: linear-gradient(
    90deg,
    rgba(64, 223, 159, 0.15),
    rgba(59, 130, 246, 0.15)
  );
  color: #40DF9F;
  position: relative;
  overflow: hidden;
}

.nav-link.active::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    90deg,
    rgba(64, 223, 159, 0.1),
    rgba(59, 130, 246, 0.1)
  );
  animation: pulse 2s infinite;
}

/* Smooth Transition for Section Changes */
.nav-link {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

@keyframes pulse {
  0% {
    opacity: 0.5;
  }
  50% {
    opacity: 0.8;
  }
  100% {
    opacity: 0.5;
  }
}

/* Mobile Active Section Styles */
@media (max-width: 959px) {
  .v-list-item.active {
    background: linear-gradient(
      135deg,
      rgba(64, 223, 159, 0.15),
      rgba(59, 130, 246, 0.15)
    );
    transform: translateX(8px);
  }

  .v-list-item.active::before {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    width: 4px;
    background: linear-gradient(45deg, #40DF9F, #3B82F6);
    animation: slideIn 0.3s ease;
  }
}

@keyframes slideIn {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}

.download-btn {
  background: linear-gradient(45deg, #3B82F6, #805AD5) !important;
  border-radius: 8px;
  transition: all 0.3s ease !important;
  color: white !important;
}

.download-btn :deep(i) {
  color: white !important;
}

.download-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(59, 130, 246, 0.3);
}
</style> 