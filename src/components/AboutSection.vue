<template>
  <section id="about" class="about-section">
    <v-container class="pt-0 pb-4">
      <v-row justify="center">
        <v-col cols="12" md="10">
          <!-- Enhanced Header with Parallax Effect -->
          <div class="section-header mt-0" v-parallax="0.3">
            <TransitionGroup 
              name="stagger"
              tag="div"
              :css="false"
              @before-enter="beforeEnter"
              @enter="enter"
            >
              <span key="overline" class="text-overline text-primary mb-2 d-block">ABOUT ME</span>
              <h2 key="title" class="text-h3 font-weight-bold">
                My <span class="gradient-text">Professional Journey</span>
              </h2>
              <div key="subtitle" class="text-subtitle-1 mt-4">
                Building the future through technology and leadership
              </div>
            </TransitionGroup>
          </div>

          <!-- Enhanced Tabs with Progress Indicator -->
          <v-tabs
            v-model="activeTab"
            color="primary"
            align-tabs="center"
            class="mb-8 fade-up glass-tabs"
            :height="64"
          >
            <v-tab value="education" class="tab-item">
              <v-icon left>mdi-school</v-icon>
              Education
            </v-tab>
            <v-tab value="experience" class="tab-item">
              <v-icon left>mdi-briefcase</v-icon>
              Experience
            </v-tab>
            <v-tab value="leadership" class="tab-item">
              <v-icon left>mdi-account-group</v-icon>
              Leadership
            </v-tab>
            <template v-slot:progress>
              <v-progress-linear
                :model-value="progressValue"
                color="primary"
                height="3"
                absolute
              ></v-progress-linear>
            </template>
          </v-tabs>

          <!-- Enhanced Window with Swipe Gestures -->
          <v-window v-model="activeTab" class="fade-up" touchless>
            <v-window-item value="education">
              <v-timeline side="end" align="start" class="animated-timeline">
                <v-timeline-item
                  v-for="(edu, index) in education"
                  :key="edu.period"
                  :dot-color="edu.color"
                  size="small"
                  :data-index="index"
                >
                  <template v-slot:opposite>
                    <div class="text-caption">{{ edu.period }}</div>
                  </template>
                  <v-card class="elevation-1 hover-card glass-effect">
                    <v-card-title class="text-h6">{{ edu.title }}</v-card-title>
                    <v-card-text>{{ edu.institution }}</v-card-text>
                    <v-card-actions v-if="edu.details">
                      <v-btn
                        color="primary"
                        variant="text"
                        @click="toggleDetails(edu)"
                      >
                        {{ edu.showDetails ? 'Hide' : 'Show' }} Details
                      </v-btn>
                    </v-card-actions>
                    <v-expand-transition>
                      <div v-if="edu.showDetails">
                        <v-divider></v-divider>
                        <v-card-text>{{ edu.details }}</v-card-text>
                      </div>
                    </v-expand-transition>
                  </v-card>
                </v-timeline-item>
              </v-timeline>
            </v-window-item>

            <!-- Experience Tab -->
            <v-window-item value="experience">
              <v-timeline side="end" align="start" class="animated-timeline">
                <v-timeline-item
                  v-for="(exp, index) in experience"
                  :key="exp.company"
                  :dot-color="exp.color"
                  size="small"
                  :data-index="index"
                >
                  <template v-slot:opposite>
                    <div class="text-caption">{{ exp.period }}</div>
                  </template>
                  <v-card class="elevation-1 hover-card glass-effect">
                    <v-card-title class="text-h6">{{ exp.role }}</v-card-title>
                    <v-card-subtitle>{{ exp.company }}</v-card-subtitle>
                    <v-card-text>{{ exp.description }}</v-card-text>
                    <v-card-actions v-if="exp.details">
                      <v-btn
                        color="primary"
                        variant="text"
                        @click="toggleDetails(exp)"
                      >
                        {{ exp.showDetails ? 'Hide' : 'Show' }} Details
                      </v-btn>
                    </v-card-actions>
                    <v-expand-transition>
                      <div v-if="exp.showDetails">
                        <v-divider></v-divider>
                        <v-card-text>{{ exp.details }}</v-card-text>
                      </div>
                    </v-expand-transition>
                  </v-card>
                </v-timeline-item>
              </v-timeline>
            </v-window-item>

            <!-- Leadership Tab -->
            <v-window-item value="leadership">
              <v-timeline side="end" align="start" class="animated-timeline">
                <v-timeline-item
                  v-for="(role, index) in leadership"
                  :key="role.period"
                  :dot-color="role.color"
                  size="small"
                  :data-index="index"
                >
                  <template v-slot:opposite>
                    <div class="text-caption">{{ role.period }}</div>
                  </template>
                  <v-card class="elevation-1 hover-card glass-effect">
                    <v-card-title class="text-h6">{{ role.title }}</v-card-title>
                    <v-card-subtitle>{{ role.organization }}</v-card-subtitle>
                    <v-card-text>{{ role.description }}</v-card-text>
                    <v-card-actions v-if="role.details">
                      <v-btn
                        color="primary"
                        variant="text"
                        @click="toggleDetails(role)"
                      >
                        {{ role.showDetails ? 'Hide' : 'Show' }} Details
                      </v-btn>
                    </v-card-actions>
                    <v-expand-transition>
                      <div v-if="role.showDetails">
                        <v-divider></v-divider>
                        <v-card-text>{{ role.details }}</v-card-text>
                      </div>
                    </v-expand-transition>
                  </v-card>
                </v-timeline-item>
              </v-timeline>
            </v-window-item>
          </v-window>
        </v-col>
      </v-row>
    </v-container>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

// Register ScrollTrigger plugin
gsap.registerPlugin(ScrollTrigger)

const activeTab = ref('education')

// Enhanced Education Data
const education = ref([
  {
    period: '2023 - Present',
    title: 'Engineering Studies',
    institution: 'First Year Engineering Student',
    color: 'primary',
    details: 'Specializing in software engineering and system architecture...',
    showDetails: false
  },
  {
    period: '2020 - 2023',
    title: 'Bachelor\'s Degree',
    institution: 'University',
    color: 'secondary'
  },
  {
    period: '2020',
    title: 'High School Diploma',
    institution: 'High School',
    color: 'info'
  }
])

const experience = ref([
  {
    period: '08/2024 - Present',
    role: 'Software Developer',
    company: 'Next Consult',
    description: 'Full-time software development position',
    color: 'primary',
    details: 'Working on enterprise software solutions, focusing on web development and system architecture.',
    showDetails: false
  },
  {
    period: '2023',
    role: 'Intern',
    company: 'Orange Digital Center',
    description: '4-month internship experience',
    color: 'deep-orange',
    details: 'Participated in digital transformation projects and mobile app development initiatives.',
    showDetails: false
  },
  {
    period: '2023',
    role: 'Intern',
    company: 'Nokia',
    description: '3-month internship experience',
    color: 'blue',
    details: 'Worked on telecommunications software and network management systems.',
    showDetails: false
  },
  {
    period: '2023',
    role: 'Intern',
    company: 'Tunisair',
    description: '1-month internship experience',
    color: 'red',
    details: 'Gained experience in aviation software systems and database management.',
    showDetails: false
  }
])

const leadership = ref([
  {
    period: '2024 - Present',
    title: 'Student Chapter Coordinator',
    organization: 'PES Tunisia Section',
    description: 'Coordinating student chapters across Tunisia',
    color: 'primary',
    details: 'Leading initiatives to strengthen collaboration between student chapters and promote professional development.',
    showDetails: false
  },
  {
    period: '2023',
    title: 'IEEE ISSATM SB Chair',
    organization: 'IEEE',
    description: 'Leading the Student Branch',
    color: 'secondary',
    details: 'Organized technical workshops, conferences, and networking events for engineering students.',
    showDetails: false
  },
  {
    period: '2022',
    title: 'IAS ISSATM SBC Chair',
    organization: 'IEEE',
    description: 'Industry Applications Society Chapter leadership',
    color: 'info',
    details: 'Facilitated industry connections and organized professional development activities.',
    showDetails: false
  },
  {
    period: '2021 - 2022',
    title: 'IEEE Member',
    organization: 'IEEE',
    description: 'Active member participation',
    color: 'success',
    details: 'Participated in technical projects and community outreach programs.',
    showDetails: false
  }
])

// Computed Progress Value
const progressValue = computed(() => {
  const tabs = ['education', 'experience', 'leadership']
  const index = tabs.indexOf(activeTab.value)
  return ((index + 1) / tabs.length) * 100
})

// Enhanced Animation Handlers
const beforeEnter = (el) => {
  gsap.set(el, {
    opacity: 0,
    y: 50,
    scale: 0.95
  })
}

const enter = (el, done) => {
  gsap.to(el, {
    opacity: 1,
    y: 0,
    scale: 1,
    duration: 0.8,
    delay: el.dataset.index * 0.2,
    ease: 'power3.out',
    onComplete: done
  })
}

// Parallax Directive
const vParallax = {
  mounted(el, binding) {
    const speed = binding.value || 0.1
    gsap.to(el, {
      yPercent: speed * 100,
      scrollTrigger: {
        trigger: el,
        scrub: true
      }
    })
  }
}

// Toggle Details Function
const toggleDetails = (item) => {
  item.showDetails = !item.showDetails
}
</script>

<style lang="scss" scoped>
.about-section {
  position: relative;
  overflow: hidden;
  margin-top: 0;
  padding-top: 0.5rem;
  
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 100px;
    background: linear-gradient(180deg, rgba(var(--v-theme-surface), 0.05) 0%, transparent 100%);
    pointer-events: none;
  }
}

.section-header {
  text-align: center;
  margin-bottom: 1rem;
  position: relative;
  padding-top: 1rem;

  &::after {
    content: '';
    position: absolute;
    bottom: -20px;
    left: 50%;
    transform: translateX(-50%);
    width: 60px;
    height: 4px;
    background: linear-gradient(90deg, #40DF9F, #3B82F6);
    border-radius: 2px;
  }
}

// Enhanced Timeline Styles
.animated-timeline {
  :deep(.v-timeline-item__body) {
    max-width: 100%;
    transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  }

  :deep(.v-timeline-item) {
    opacity: 0;
    transform: translateY(20px);
    animation: fadeInUp 0.6s ease forwards;
    animation-delay: calc(var(--index) * 0.2s);
  }

  :deep(.v-timeline-item__dot) {
    transition: all 0.3s ease;
    
    &::before {
      content: '';
      position: absolute;
      inset: -4px;
      border-radius: 50%;
      background: linear-gradient(45deg, #40DF9F, #3B82F6);
      opacity: 0;
      transition: opacity 0.3s ease;
    }
  }

  :deep(.v-timeline-item:hover .v-timeline-item__dot) {
    transform: scale(1.2);
    
    &::before {
      opacity: 0.3;
    }
  }
}

// Enhanced Tab Styles
.tab-item {
  background: rgba(255, 255, 255, 0.03);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);

  &:hover {
    background: rgba(64, 223, 159, 0.1);
    transform: translateY(-2px);
    border-color: rgba(64, 223, 159, 0.3);
  }
}

// Enhanced Gradient Text Animation
.gradient-text {
  background: linear-gradient(
    45deg,
    #40DF9F,
    #3B82F6,
    #40DF9F
  );
  background-size: 200% auto;
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  animation: gradientText 6s linear infinite;
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

// Enhanced Card Hover Effects
.hover-card {
  &::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(45deg, #40DF9F20, #3B82F620);
    opacity: 0;
    transition: opacity 0.3s ease;
  }

  &:hover::before {
    opacity: 1;
  }

  .v-card-title {
    background: linear-gradient(45deg, #40DF9F, #3B82F6);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    transition: all 0.3s ease;
  }
}

// Enhanced Tabs Styles
.glass-tabs {
  :deep(.v-tab) {
    position: relative;
    overflow: hidden;
    
    &::before {
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
    
    &:hover::before {
      transform: translateX(100%);
    }
  }

  :deep(.v-tab--selected) {
    background: linear-gradient(
      45deg,
      rgba(64, 223, 159, 0.1),
      rgba(59, 130, 246, 0.1)
    );
    border-color: rgba(64, 223, 159, 0.4);
  }
}

// Animations
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

// Responsive Adjustments
@media (max-width: 960px) {
  .section-header {
    padding-top: 0;
  }
  
  .tab-item {
    font-size: 0.875rem;
  }

  .hover-card {
    &:hover {
      transform: translateX(-2px) scale(1.01);
    }
  }

  .animated-timeline {
    :deep(.v-timeline-item__dot) {
      &:hover {
        transform: scale(1.1);
      }
    }
  }
}

/* Dark Mode Optimizations */
@media (prefers-color-scheme: dark) {
  .glass-effect {
    background: rgba(255, 255, 255, 0.02);
  }

  .hover-card:hover {
    background: rgba(64, 223, 159, 0.05);
  }
}

/* Print Styles */
@media print {
  .animated-timeline {
    :deep(.v-timeline-item) {
      opacity: 1;
      transform: none;
      animation: none;
    }
  }

  .glass-effect {
    background: white;
    backdrop-filter: none;
    box-shadow: none;
    border: 1px solid #eee;
  }
}
</style> 