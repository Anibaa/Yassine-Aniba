<template>
  <div class="hero" id="home">
    <!-- Updated animated background -->
    <div class="animated-bg">
      <div class="particles-container">
        <div v-for="n in 20" :key="n" class="particle"></div>
      </div>
      <div class="gradient-sphere"></div>
      <div class="gradient-orb"></div>
      <div class="gradient-orb-2"></div>
      <div class="noise-overlay"></div>
      <div class="grid-overlay"></div>
    </div>
    
    <div class="content-container">
      <!-- Left Side -->
      <div class="hero-left">
        <div class="status-badge">
          <div class="status-glow"></div>
          <span class="status-icon">
            <span class="pulse-ring"></span>
            <span class="pulse-dot"></span>
          </span>
          <span class="status-text">Available for new projects</span>
        </div>

        <h1 class="headline">
          Building
          <span class="accent-text">Digital Products</span>
          <br />
          With Excellence
        </h1>

        <div class="typewriter">
          <span class="typewriter-prefix">I specialize in</span>
          <span class="typewriter-text">{{ typewriterText }}</span>
          <span class="cursor">|</span>
        </div>

        <p class="bio">
          Crafting innovative web and mobile solutions with IoT expertise, while bringing strong 
          leadership skills and a passion for collaborative teamwork to every project.
        </p>

        <div class="cta-group">
          <button class="cta-primary" @click="scrollToSection('projects')">
            <span class="btn-text">View Projects</span>
            <span class="btn-icon">
              <v-icon>mdi-arrow-right</v-icon>
            </span>
            <div class="btn-glow"></div>
          </button>
          <button class="cta-secondary" @click="scrollToSection('contact')">
            <span class="btn-text">Let's Talk</span>
            <v-icon>mdi-message-outline</v-icon>
          </button>
        </div>

        <div class="tech-showcase">
          <h3 class="tech-title">Tech Stack</h3>
          <div class="tech-grid">
            <div 
              v-for="tech in technologies" 
              :key="tech.name" 
              class="tech-card"
              :title="tech.name"
            >
              <v-icon :icon="tech.icon" size="24" />
            </div>
          </div>
        </div>
      </div>

      <!-- Right Side -->
      <div class="hero-right">
        <div class="profile-wrapper">
          <div class="profile-card">
            <img 
              src="@/assets/profile.jpg" 
              alt="Profile" 
              class="profile-img"
            >
            <div class="card-overlay"></div>
          </div>
          <div class="experience-badge">
            <span class="exp-number">3+</span>
            <span class="exp-text">Years of<br>Experience</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Social Links -->
    <div class="social-strip">
      <a 
        v-for="social in socials" 
        :key="social.icon" 
        :href="social.link" 
        class="social-link" 
        target="_blank"
        :title="social.name"
      >
        <v-icon :icon="social.icon" />
      </a>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const scrollToSection = (sectionId) => {
  const element = document.getElementById(sectionId)
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
  }
}

const typewriterText = ref('')
const phrases = [
  'Frontend Development',
  'Web Applications',
  'Modern Solutions'
]

const technologies = [
  { name: 'Vue.js', icon: 'mdi-vuejs' },
  { name: 'React', icon: 'mdi-react' },
  { name: 'Node.js', icon: 'mdi-nodejs' },
  { name: 'TypeScript', icon: 'mdi-language-typescript' },
  { name: 'Python', icon: 'mdi-language-python' },
  { name: 'Flutter', icon: 'mdi-android' },
  { name: 'MongoDB', icon: 'mdi-database' },
  { name: 'JavaScript', icon: 'mdi-language-javascript' },
  { name: 'Firebase', icon: 'mdi-firebase' },
  { name: 'Git', icon: 'mdi-git' },
  { name: 'GitHub', icon: 'mdi-github' },
  { name: 'Angular', icon: 'mdi-angular' }
]

const socials = [
  { name: 'GitHub', icon: 'mdi-github', link: 'https://github.com/Anibaa' },
  { name: 'LinkedIn', icon: 'mdi-linkedin', link: 'https://www.linkedin.com/in/yassine-aniba-63a981224/' },
  { name: 'Email', icon: 'mdi-email', link: 'mailto:anibayassinesn@gmail.com' }
]

let currentPhraseIndex = 0
let currentCharIndex = 0
let isDeleting = false
let typingSpeed = 100

const typeWriter = () => {
  const currentPhrase = phrases[currentPhraseIndex]
  
  if (isDeleting) {
    typewriterText.value = currentPhrase.substring(0, currentCharIndex - 1)
    currentCharIndex--
  } else {
    typewriterText.value = currentPhrase.substring(0, currentCharIndex + 1)
    currentCharIndex++
  }

  if (!isDeleting && currentCharIndex === currentPhrase.length) {
    isDeleting = true
    typingSpeed = 50
    setTimeout(typeWriter, 1500)
    return
  }

  if (isDeleting && currentCharIndex === 0) {
    isDeleting = false
    currentPhraseIndex = (currentPhraseIndex + 1) % phrases.length
    typingSpeed = 100
  }

  setTimeout(typeWriter, typingSpeed)
}

onMounted(() => {
  typeWriter()
})
</script>

<style scoped>
.hero {
  min-height: 100vh;
  background: #0a0a0a;
  position: relative;
  display: flex;
  align-items: center;
  overflow: hidden;
}

.animated-bg {
  position: absolute;
  inset: 0;
  overflow: hidden;
  background: linear-gradient(
    145deg,
    rgba(10, 10, 10, 1) 0%,
    rgba(15, 15, 15, 1) 100%
  );
}

.particles-container {
  position: absolute;
  inset: 0;
  overflow: hidden;
}

.particle {
  position: absolute;
  width: 2px;
  height: 2px;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 50%;
  animation: particleFloat 20s infinite linear;
}

.gradient-sphere {
  position: absolute;
  width: 500px;
  height: 500px;
  background: radial-gradient(
    circle,
    rgba(64, 223, 159, 0.15) 0%,
    rgba(59, 130, 246, 0.15) 50%,
    transparent 70%
  );
  filter: blur(50px);
  border-radius: 50%;
  animation: sphereFloat 20s infinite ease-in-out;
  mix-blend-mode: screen;
}

.gradient-orb {
  position: absolute;
  width: 600px;
  height: 600px;
  background: radial-gradient(circle, 
    rgba(64, 223, 159, 0.15) 0%,
    rgba(64, 223, 159, 0) 70%
  );
  top: -200px;
  right: -200px;
  border-radius: 50%;
  filter: blur(50px);
  animation: float 20s infinite ease-in-out;
}

.gradient-orb-2 {
  position: absolute;
  width: 400px;
  height: 400px;
  background: radial-gradient(circle,
    rgba(99, 102, 241, 0.15) 0%,
    rgba(99, 102, 241, 0) 70%
  );
  bottom: -100px;
  left: -100px;
  border-radius: 50%;
  filter: blur(50px);
  animation: float 15s infinite ease-in-out reverse;
}

.noise-overlay {
  position: absolute;
  inset: 0;
  background-image: url('@/assets/noise.png');
  opacity: 0.05;
  mix-blend-mode: overlay;
}

.grid-overlay {
  position: absolute;
  inset: 0;
  background-image: radial-gradient(rgba(255,255,255,0.1) 1px, transparent 1px),
                    radial-gradient(rgba(255,255,255,0.1) 1px, transparent 1px);
  background-size: 30px 30px;
  background-position: 0 0, 15px 15px;
  opacity: 0.3;
}

.content-container {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 4rem;
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  gap: 2rem;
  position: relative;
  z-index: 2;
  height: 100vh;
}

.hero-left {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 2rem 0;
  height: 100%;
}

.status-badge {
  position: relative;
  overflow: hidden;
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 1.25rem;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 100px;
  backdrop-filter: blur(10px);
  margin-bottom: 2rem;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.status-badge:hover {
  background: rgba(255, 255, 255, 0.05);
  transform: translateY(-2px);
}

.status-icon {
  position: relative;
  width: 10px;
  height: 10px;
}

.pulse-dot {
  position: absolute;
  inset: 0;
  background: #40DF9F;
  border-radius: 50%;
}

.pulse-ring {
  position: absolute;
  inset: -2px;
  border: 2px solid #40DF9F;
  border-radius: 50%;
  animation: pulse 2s infinite;
}

.status-text {
  color: rgba(255, 255, 255, 0.9);
  font-size: 0.9rem;
  font-weight: 500;
}

.headline {
  font-size: clamp(2rem, 4vw, 3.5rem);
  font-weight: 700;
  line-height: 1.1;
  color: white;
  margin-bottom: 1.5rem;
  letter-spacing: -0.02em;
}

.accent-text {
  background: linear-gradient(45deg, #40DF9F, #3B82F6);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

.typewriter {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.typewriter-prefix {
  color: rgba(255, 255, 255, 0.6);
}

.typewriter-text {
  color: white;
  font-weight: 500;
}

.cursor {
  color: #40DF9F;
  animation: blink 1s infinite;
}

.bio {
  color: rgba(255, 255, 255, 0.7);
  font-size: 1rem;
  line-height: 1.5;
  max-width: 500px;
  margin-bottom: 2rem;
}

.cta-group {
  display: flex;
  gap: 1.5rem;
  margin-bottom: 0.5rem;
}

.cta-primary {
  position: relative;
  background: linear-gradient(135deg, #40DF9F 0%, #3B82F6 100%);
  color: #000;
  padding: 1rem 2.5rem;
  border: none;
  border-radius: 12px;
  font-weight: 600;
  font-size: 1rem;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  overflow: hidden;
  isolation: isolate;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.cta-primary::before {
  content: '';
  position: absolute;
  inset: 1px;
  background: #fff;
  border-radius: 11px;
  z-index: -1;
  transition: all 0.4s ease;
}

.cta-primary::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    45deg,
    transparent,
    rgba(255, 255, 255, 0.4),
    transparent
  );
  transform: translateX(-100%);
  transition: transform 0.6s ease;
}

.cta-primary:hover {
  transform: translateY(-3px);
  box-shadow: 
    0 10px 20px rgba(64, 223, 159, 0.2),
    0 6px 6px rgba(59, 130, 246, 0.1),
    0 0 0 2px rgba(64, 223, 159, 0.3);
}

.cta-primary:hover::before {
  background: linear-gradient(135deg, #40DF9F 0%, #3B82F6 100%);
  opacity: 0.9;
}

.cta-primary:hover::after {
  transform: translateX(100%);
}

.cta-primary:active {
  transform: translateY(-1px);
}

.btn-text {
  position: relative;
  z-index: 1;
  transition: all 0.3s ease;
}

.btn-icon {
  position: relative;
  z-index: 1;
  transition: all 0.3s ease;
}

.cta-primary:hover .btn-icon {
  transform: translateX(3px);
}

.cta-secondary {
  position: relative;
  background: rgba(255, 255, 255, 0.03);
  color: #fff;
  padding: 1rem 2.5rem;
  border-radius: 12px;
  font-weight: 600;
  font-size: 1rem;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  cursor: pointer;
  overflow: hidden;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.cta-secondary::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    45deg,
    transparent,
    rgba(255, 255, 255, 0.1),
    transparent
  );
  transform: translateX(-100%);
  transition: transform 0.6s ease;
}

.cta-secondary:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(64, 223, 159, 0.3);
  transform: translateY(-3px);
  box-shadow: 
    0 10px 20px rgba(0, 0, 0, 0.2),
    0 0 0 2px rgba(64, 223, 159, 0.1);
}

.cta-secondary:hover::before {
  transform: translateX(100%);
}

.cta-secondary:active {
  transform: translateY(-1px);
}

.cta-secondary:hover .btn-icon {
  transform: translateX(3px);
  color: #40DF9F;
}

.btn-glow {
  position: absolute;
  width: 50px;
  height: 100%;
  background: rgba(255, 255, 255, 0.4);
  filter: blur(5px);
  transform: skewX(-20deg);
  left: -70px;
  animation: glow 3s infinite;
}

@keyframes glow {
  0% {
    left: -70px;
    opacity: 0;
  }
  50% {
    opacity: 0.5;
  }
  100% {
    left: 150%;
    opacity: 0;
  }
}

.tech-showcase {
  margin-top: 2rem;
}

.tech-title {
  color: rgba(255,255,255,0.6);
  font-size: 0.9rem;
  font-weight: 500;
  margin-bottom: 1rem;
}

.tech-grid {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 0.75rem;
  max-width: 350px;
}

.tech-card {
  position: relative;
  overflow: hidden;
  backdrop-filter: blur(10px);
  width: 45px;
  height: 45px;
  background: rgba(255, 255, 255, 0.03);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.tech-card::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(45deg, 
    rgba(64, 223, 159, 0.1),
    rgba(59, 130, 246, 0.1)
  );
  opacity: 0;
  transition: opacity 0.3s ease;
}

.tech-card:hover::before {
  opacity: 1;
}

.tech-card:hover {
  transform: translateY(-5px) scale(1.1);
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(64, 223, 159, 0.3);
}

.hero-right {
  display: flex;
  align-items: center;
  justify-content: center;
}

.profile-wrapper {
  position: relative;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem 0;
}

.profile-card {
  position: relative;
  width: 320px;
  height: 420px;
  border-radius: 24px;
  overflow: hidden;
  isolation: isolate;
}

.profile-card::before {
  content: '';
  position: absolute;
  inset: -1px;
  background: linear-gradient(
    45deg,
    rgba(64, 223, 159, 0.6),
    rgba(59, 130, 246, 0.6)
  );
  border-radius: inherit;
  z-index: 1;
  opacity: 0.7;
}

.profile-card::after {
  content: '';
  position: absolute;
  inset: 1px;
  background: #0a0a0a;
  border-radius: inherit;
  z-index: 1;
}

.profile-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  border-radius: inherit;
  position: relative;
  z-index: 2;
  transition: all 0.4s ease;
}

.experience-badge {
  position: absolute;
  bottom: 55px;
  right: -25px;
  padding: 0.75rem;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  backdrop-filter: blur(10px);
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.exp-number {
  font-size: 1.5rem;
  font-weight: 600;
  color: #40DF9F;
}

.exp-text {
  color: rgba(255, 255, 255, 0.9);
  font-size: 0.9rem;
  font-weight: 500;
  text-align: right;
}

.social-strip {
  position: fixed;
  left: 1rem;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  z-index: 10;
}

.social-link {
  color: #fff;
  opacity: 0.6;
  transition: all 0.3s ease;
}

.social-link:hover {
  opacity: 1;
  color: #00ff88;
  transform: translateX(5px);
}

@keyframes pulse {
  0% { opacity: 0.5; }
  50% { opacity: 1; }
  100% { opacity: 0.5; }
}

@keyframes meshFloat {
  0% { transform: translateY(0); }
  50% { transform: translateY(-20px); }
  100% { transform: translateY(0); }
}

@keyframes float {
  0%, 100% { transform: translate(0, 0) rotate(0deg); }
  25% { transform: translate(10px, -10px) rotate(2deg); }
  50% { transform: translate(-5px, -20px) rotate(-1deg); }
  75% { transform: translate(-15px, -5px) rotate(-3deg); }
}

@keyframes particleFloat {
  0% {
    transform: translate(random(-100, 100) * 1%, random(-100, 100) * 1%);
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    transform: translate(random(-100, 100) * 1%, random(-100, 100) * 1%);
    opacity: 0;
  }
}

@keyframes sphereFloat {
  0%, 100% {
    transform: translate(0, 0) scale(1);
    opacity: 0.5;
  }
  50% {
    transform: translate(-30px, -30px) scale(1.2);
    opacity: 0.7;
  }
}

@keyframes statusGlow {
  0% { left: -100%; }
  50%, 100% { left: 200%; }
}

@keyframes borderGlow {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.6;
  }
}

@media (max-width: 1280px) {
  .content-container {
    padding: 0 3rem;
  }

  .profile-card {
    width: 280px;
    height: 380px;
  }
}

@media (max-width: 1024px) {
  .content-container {
    grid-template-columns: 1fr;
    text-align: center;
    padding: 0 2rem;
    height: auto;
    min-height: 100vh;
  }

  .hero-left {
    padding: 4rem 0;
  }

  .tech-grid {
    margin: 0 auto;
  }

  .hero-right {
    display: none;
  }

  .social-strip {
    display: none;
  }
}

@media (max-width: 768px) {
  .profile-wrapper {
    padding: 1rem 0;
  }

  .profile-card {
    width: 240px;
    height: 320px;
  }

  .social-strip {
    padding: 1.5rem 0;
    gap: 1.5rem;
  }

  .tech-card:hover {
    transform: translateY(-3px) scale(1.05);
  }
  
  .social-link {
    width: 40px;
    height: 40px;
  }
}

@media (max-width: 480px) {
  .profile-card {
    width: 200px;
    height: 260px;
  }

  .social-strip {
    padding: 1rem 0;
    gap: 1.25rem;
  }

  .social-link {
    font-size: 1.25rem;
  }

  .cta-primary,
  .cta-secondary {
    padding: 0.875rem 1.75rem;
  }

  .social-link {
    width: 35px;
    height: 35px;
  }
}

@media (max-height: 800px) {
  .status-badge {
    margin-bottom: 1.5rem;
  }

  .headline {
    font-size: clamp(1.8rem, 3vw, 3rem);
    margin-bottom: 1rem;
  }

  .bio {
    margin-bottom: 1.5rem;
  }

  .tech-showcase {
    margin-top: 1.5rem;
  }

  .tech-card {
    width: 40px;
    height: 40px;
  }
}

/* Enhanced Glass Morphism Effects */
.glass-effect {
  background: rgba(255, 255, 255, 0.03);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 
    0 4px 15px rgba(0, 0, 0, 0.1),
    inset 0 0 20px rgba(255, 255, 255, 0.05);
}

/* Advanced Background Animation */
.animated-bg::before {
  content: '';
  position: absolute;
  inset: -50%;
  background: radial-gradient(
    circle at center,
    rgba(64, 223, 159, 0.1) 0%,
    rgba(59, 130, 246, 0.1) 25%,
    transparent 50%
  );
  animation: rotateGradient 20s linear infinite;
}

/* Enhanced Profile Card */
.profile-card {
  position: relative;
  isolation: isolate;
}

.profile-card::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    180deg,
    transparent 0%,
    rgba(64, 223, 159, 0.1) 100%
  );
  z-index: 3;
}

.profile-card:hover .profile-img {
  transform: scale(1.05);
  filter: brightness(1.1);
}

/* Advanced Tech Cards */
.tech-card {
  position: relative;
  overflow: hidden;
}

.tech-card::before {
  content: '';
  position: absolute;
  inset: -100%;
  background: linear-gradient(
    45deg,
    transparent,
    rgba(255, 255, 255, 0.1),
    transparent
  );
  transition: transform 0.6s;
}

.tech-card:hover::before {
  transform: translateX(200%);
}

/* Enhanced CTA Buttons */
.cta-primary {
  position: relative;
  overflow: hidden;
}

.cta-primary::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    45deg,
    transparent,
    rgba(255, 255, 255, 0.2),
    transparent
  );
  transform: translateX(-100%);
  transition: transform 0.6s;
}

.cta-primary:hover::after {
  transform: translateX(100%);
}

/* Advanced Status Badge */
.status-badge {
  position: relative;
  overflow: hidden;
}

.status-glow {
  position: absolute;
  width: 100%;
  height: 100%;
  background: radial-gradient(
    circle at center,
    rgba(64, 223, 159, 0.2),
    transparent 70%
  );
  animation: pulseGlow 2s infinite;
}

/* New Animations */
@keyframes rotateGradient {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes pulseGlow {
  0%, 100% {
    opacity: 0.5;
    transform: scale(1);
  }
  50% {
    opacity: 1;
    transform: scale(1.1);
  }
}

/* Enhanced Hover Effects */
.headline .accent-text {
  position: relative;
  display: inline-block;
}

.headline .accent-text::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: -2px;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, #40DF9F, #3B82F6);
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.3s ease;
}

.headline:hover .accent-text::after {
  transform: scaleX(1);
  transform-origin: left;
}

/* Advanced Social Links */
.social-link {
  position: relative;
  overflow: hidden;
}

.social-link::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(45deg, #40DF9F, #3B82F6);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.social-link:hover::before {
  opacity: 0.2;
}

.social-link::after {
  content: '';
  position: absolute;
  width: 15px;
  height: 100%;
  background: rgba(255, 255, 255, 0.2);
  transform: skewX(-20deg) translateX(-50px);
  transition: transform 0.6s;
}

.social-link:hover::after {
  transform: skewX(-20deg) translateX(150px);
}

/* New Responsive Enhancements */
@media (max-width: 1024px) {
  .glass-effect {
    backdrop-filter: blur(8px);
  }

  .profile-card::after {
    background: linear-gradient(
      180deg,
      transparent 0%,
      rgba(64, 223, 159, 0.15) 100%
    );
  }
}

@media (max-width: 768px) {
  .tech-card::before {
    display: none;
  }

  .social-link::after {
    width: 10px;
  }
}

@media (prefers-reduced-motion: reduce) {
  .animated-bg::before,
  .tech-card::before,
  .social-link::after,
  .cta-primary::after {
    display: none;
  }
}

/* Elegant Gradient Overlays */
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(
    circle at 50% 50%,
    rgba(64, 223, 159, 0.08),
    rgba(59, 130, 246, 0.08),
    transparent 60%
  );
  animation: subtleBreathing 10s infinite alternate;
}

/* Enhanced Profile Card */
.profile-card {
  position: relative;
  isolation: isolate;
}

.profile-card::before {
  content: '';
  position: absolute;
  inset: -2px;
  background: conic-gradient(
    from 0deg,
    #40DF9F,
    #3B82F6,
    #40DF9F,
    #3B82F6,
    #40DF9F
  );
  border-radius: 32px;
  animation: borderRotate 8s linear infinite;
  z-index: 1;
}

/* Refined Status Badge */
.status-badge {
  background: linear-gradient(
    135deg,
    rgba(255, 255, 255, 0.05),
    rgba(255, 255, 255, 0.02)
  );
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.status-badge::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    45deg,
    transparent,
    rgba(255, 255, 255, 0.1),
    transparent
  );
  transform: translateX(-100%);
  transition: transform 0.6s;
}

.status-badge:hover::before {
  transform: translateX(100%);
}

/* Sophisticated Tech Cards */
.tech-card {
  background: rgba(255, 255, 255, 0.03);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.tech-card:hover {
  border-color: rgba(64, 223, 159, 0.5);
  box-shadow: 
    0 0 20px rgba(64, 223, 159, 0.2),
    inset 0 0 15px rgba(64, 223, 159, 0.1);
  transform: translateY(-5px) scale(1.05);
}

/* Elegant CTA Buttons */
.cta-primary {
  background: linear-gradient(
    135deg,
    #40DF9F,
    #3B82F6
  );
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.cta-primary:hover {
  box-shadow: 
    0 10px 20px rgba(64, 223, 159, 0.2),
    0 6px 6px rgba(59, 130, 246, 0.1),
    inset 0 0 10px rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
}

/* Refined Typography */
.headline {
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.accent-text {
  background: linear-gradient(
    45deg,
    #40DF9F,
    #3B82F6,
    #40DF9F
  );
  background-size: 200% auto;
  -webkit-background-clip: text;
  background-clip: text;
  animation: gradientText 6s linear infinite;
}

/* Enhanced Social Links */
.social-link {
  background: rgba(255, 255, 255, 0.03);
  backdrop-filter: blur(12px);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.social-link:hover {
  background: rgba(64, 223, 159, 0.1);
  transform: translateY(-3px);
  box-shadow: 
    0 5px 15px rgba(64, 223, 159, 0.2),
    inset 0 0 10px rgba(64, 223, 159, 0.1);
}

/* New Elegant Animations */
@keyframes subtleBreathing {
  0%, 100% {
    opacity: 0.5;
    transform: scale(1);
  }
  50% {
    opacity: 0.8;
    transform: scale(1.1);
  }
}

@keyframes borderRotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

@keyframes gradientText {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

/* Refined Hover States */
.tech-showcase:hover .tech-card {
  opacity: 0.5;
  transform: scale(0.95);
}

.tech-showcase .tech-card:hover {
  opacity: 1;
  transform: scale(1.05);
}

/* Enhanced Media Queries */
@media (max-width: 1024px) {
  .profile-card::before {
    animation-duration: 12s;
  }
  
  .accent-text {
    animation-duration: 8s;
  }
}

@media (max-width: 768px) {
  .tech-card {
    backdrop-filter: blur(8px);
  }
  
  .social-link:hover {
    transform: translateY(-2px);
  }
}

@media (prefers-reduced-motion: reduce) {
  .profile-card::before,
  .accent-text,
  .status-badge::before {
    animation: none;
  }
}

/* Glass Morphism Enhancement */
.glass-morphism {
  background: rgba(255, 255, 255, 0.03);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 
    0 4px 15px rgba(0, 0, 0, 0.1),
    inset 0 0 20px rgba(255, 255, 255, 0.05);
}

/* Smooth Scrolling */
html {
  scroll-behavior: smooth;
}

/* Enhanced Focus States */
*:focus {
  outline: 2px solid rgba(64, 223, 159, 0.5);
  outline-offset: 2px;
}

/* Elegant Loading States */
.loading {
  position: relative;
  overflow: hidden;
}

.loading::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.1),
    transparent
  );
  transform: translateX(-100%);
  animation: shimmer 1.5s infinite;
}

@keyframes shimmer {
  100% {
    transform: translateX(100%);
  }
}

/* Responsive Adjustments */
@media (max-width: 768px) {
  .cta-group {
    flex-direction: column;
    gap: 1rem;
    width: 100%;
    max-width: 300px;
    margin: 0 auto 0.5rem auto;
  }

  .cta-primary,
  .cta-secondary {
    width: 100%;
    justify-content: center;
    padding: 0.875rem 2rem;
  }
}

@media (hover: none) {
  .cta-primary:hover,
  .cta-secondary:hover {
    transform: none;
    box-shadow: none;
  }

  .btn-glow {
    display: none;
  }
}

/* Active States */
.cta-primary:focus-visible,
.cta-secondary:focus-visible {
  outline: 2px solid rgba(64, 223, 159, 0.5);
  outline-offset: 2px;
}

.cta-primary:active,
.cta-secondary:active {
  transform: translateY(1px);
  transition: transform 0.1s;
}
</style>