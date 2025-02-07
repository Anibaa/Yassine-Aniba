<template>
  <div id="projects" class="projects-section">
    <v-parallax
      src="@/assets/images/projects-bg.jpg"
      class="project-parallax"
    >
      <div class="overlay"></div>
      <div class="section-content py-24">
        <v-row justify="center">
          <v-col cols="12" lg="10" xl="8">
            <!-- Section Header -->
            <div class="text-center mb-24" v-intersect="onIntersect">
              <div class="d-flex align-center justify-center mb-6">
                <div class="section-line"></div>
                <span class="text-overline text-primary fade-up px-6">EXPERTISE & PROJECTS</span>
                <div class="section-line"></div>
              </div>
              <h2 class="text-h2 font-weight-bold text-white mb-6 fade-up">Creative Solutions</h2>
              <div class="text-subtitle-1 text-grey-lighten-2 mx-auto fade-up" style="max-width: 700px">
                Crafting innovative digital experiences through code and design
              </div>
            </div>

            <!-- Expertise Stats -->
            <v-row class="mb-24">
              <v-col v-for="(stat, index) in expertiseStats" :key="stat.title" cols="12" sm="6" md="3">
                <div class="expertise-card fade-up" :style="{ animationDelay: `${index * 0.2}s` }">
                  <div class="expertise-icon">
                    <v-icon :icon="stat.icon" color="primary" size="32"></v-icon>
                  </div>
                  <div class="expertise-count text-h4 font-weight-bold">{{ stat.count }}</div>
                  <div class="expertise-title text-grey-lighten-2">{{ stat.title }}</div>
                </div>
              </v-col>
            </v-row>

            <!-- Project Categories -->
            <v-slide-group
              v-model="selectedCategory"
              selected-class="bg-primary"
              show-arrows
              class="custom-slide-group mb-1"
            >
              <v-slide-group-item
                v-for="category in categories"
                :key="category.name"
                v-slot="{ isSelected, toggle }"
              >
                <v-btn
                  :color="isSelected ? 'primary' : 'surface'"
                  class="ma-2 category-btn"
                  :class="{ 'selected': category.name === selectedCategory }"
                  @click="handleCategoryChange(category.name)"
                  variant="flat"
                  rounded="pill"
                >
                  <v-icon :icon="category.icon" start size="18"></v-icon>
                  {{ category.name }}
                  <span class="category-count" v-if="getCategoryCount(category.name)">
                    {{ getCategoryCount(category.name) }}
                  </span>
                </v-btn>
              </v-slide-group-item>
            </v-slide-group>

            <!-- Projects Grid -->
            <v-row class="projects-grid">
              <v-col
                v-for="(project, index) in filteredProjects"
                :key="project.id"
                cols="12"
                md="6"
                lg="6"
                class="project-item fade-up pa-4"
                :style="{ animationDelay: `${index * 0.2}s` }"
              >
                <v-hover v-slot="{ isHovering, props }">
                  <v-card
                    v-bind="props"
                    :elevation="isHovering ? 16 : 4"
                    class="project-card"
                    :class="{ 'on-hover': isHovering }"
                  >
                    <div class="project-content-wrapper pa-8">
                      <div class="project-header">
                        <div class="project-badge">{{ project.category }}</div>
                        <v-chip
                          v-if="project.confidential"
                          color="error"
                          size="small"
                          variant="flat"
                        >
                          Confidential
                        </v-chip>
                      </div>
                      
                      <h3 class="project-title">{{ project.title }}</h3>
                      <p class="project-description">{{ project.description }}</p>
                      
                      <div class="tech-stack">
                        <div class="tech-grid">
                          <v-chip
                            v-for="(tech, index) in project.technologies"
                            :key="tech"
                            size="small"
                            variant="flat"
                            color="primary"
                            class="tech-chip"
                            :style="{ '--chip-index': index }"
                          >
                            {{ tech }}
                          </v-chip>
                        </div>
                      </div>
                    </div>
                  </v-card>
                </v-hover>
              </v-col>
            </v-row>

            <!-- See More Button -->
            <div class="text-center mt-12 mb-6" v-if="hasMoreProjects">
              <div class="button-wrapper">
                <v-btn
                  color="primary"
                  variant="flat"
                  rounded="pill"
                  size="large"
                  @click="toggleShowAll"
                  class="see-more-btn px-6"
                  elevation="0"
                >
                  <div class="btn-content">
                    <span class="btn-text">{{ visibleCount.value <= 3 ? 'View More' : 'Load More' }}</span>
                    <div class="icon-wrapper">
                      <v-icon
                        icon="mdi-chevron-down"
                        class="toggle-icon"
                        size="18"
                      ></v-icon>
                    </div>
                  </div>
                  <div class="btn-background"></div>
                </v-btn>
              </div>
            </div>
            <!-- Show Less Button -->
            <div class="text-center mt-4" v-if="visibleCount.value > 3">
              <v-btn
                color="primary"
                variant="text"
                size="small"
                @click="visibleCount = 3"
                class="show-less-btn"
              >
                <v-icon
                  icon="mdi-chevron-up"
                  class="mr-1"
                  size="16"
                ></v-icon>
                Show Less
              </v-btn>
            </div>
          </v-col>
        </v-row>
      </div>
    </v-parallax>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const selectedCategory = ref('All')
const showAll = ref(false)
const visibleCount = ref(3)

const expertiseStats = [
  { icon: 'mdi-code-tags', count: '20+', title: 'Projects Completed' },
  { icon: 'mdi-account-group', count: '10+', title: 'Happy Clients' },
  { icon: 'mdi-star', count: '98%', title: 'Success Rate' },
  { icon: 'mdi-certificate', count: '3+', title: 'Years Experience' },
]

const categories = [
  { name: 'All', icon: 'mdi-apps' },
  { name: 'Web Development', icon: 'mdi-web' },
  { name: 'Mobile Apps', icon: 'mdi-cellphone' },
  { name: 'Backend', icon: 'mdi-database' },
]

const projects = [
  {
    id: 1,
    title: 'Diagnostics Management Platform',
    description: 'Designed a multi-tenant SaaS for managing spare parts inventory, client diagnostics, and technician workflows. Admin panel: CRUD operations for products, spare parts, models, and stock management. Technician module: Diagnostics ticketing system linked to client requests, with progress tracking and reporting. Client portal: Automated account creation, real-time updates on diagnostics, and notication system. Super Admin dashboard for multi-tenancy management, access control, and permission handling. Backend developed using Node.js (Express.js) and MongoDB, with Redis caching for performance optimization.',
    technologies: ['Vue.js', 'Node.js', 'MongoDB', 'Express.js'],
    category: 'Web Development',
    confidential: true,
  },
  {
    id: 2,
    title: 'Carbon Footprint Management API Developed',
    description: 'Documented a REST API for carbon footprint assessment in enterprises. Implemented data aggregation, analytics, and reporting for sustainability tracking. Created interactive dashboards with real-time data visualization using D3.js and Chart.js. Integrated JWT authentication, rate limiting, and role-based access control (RBAC) for security',
    technologies: ['Node.js', 'MongoDB', 'Express.js'],
    category: 'Backend',
    confidential: true,
  },
  {
    id: 3,
    title: 'Intervention App',
    description: 'Developed a task management system for field technicians to track and manage assigned interventions. Features include real-time task assignment tracking, detailed intervention reporting, issue logging and documentation, and full traceability of technician activities. Implemented data collection forms for capturing intervention details, status updates, and completion metrics. Built-in notification system keeps technicians informed of new assignments and updates.',
    technologies: ['Flutter', 'Node.js', 'MongoDB', 'Express.js'],
    category: 'Mobile Apps', 
    confidential: true,
  },
  {
    id: 4,
    title: 'Big Deals App',
    description: 'Implemented a subscription-based e-commerce and discount aggregation platform using Flutter and Node.js backend. Integrated secure payment processing, push notifications, and dynamic deal allocation with API integration for national businesses (cinemas, restaurants, etc.). Backend services deployed with Express.js and MongoDB, ensuring real-time transaction updates. Smart Medication Reminder App Engineered a medication management system using Flutter and Node.js, allowing users to log medications, define dosage schedules, and receive automated notifications. Implemented local storage for offline support and Firebase Cloud Messaging (FCM) for push notifications. Backend ensures secure user authentication with JWT tokens and encrypted database storage.',
    technologies: ['Flutter', 'Node.js', 'MongoDB', 'Firebase'],
    category: 'Mobile Apps',
    confidential: true,
  },
  {
    id: 5,
    title: 'Smart Agriculture',
    description: 'Developed a cross-platform mobile application using Flutter with an admin panel for managing categorized agricultural products, solutions, and recommendations. Integrated user-side features, including AI-powered chatbot, real-time weather insights, and agriculture-related news aggregation. Backend powered by Node.js and Firebase, ensuring scalable data management and secure authentication.',
    technologies: ['Flutter', 'Node.js', 'MongoDB', 'Firebase'],
    category: 'Mobile Apps',
    confidential: true,
  },
  {
    id: 6,
    title: 'Doctor-Patient Appointment System',
    description: 'Developed a dual-interface mobile application enabling patients to search for doctors via list or map, book appointments, and track medical follow-ups. The doctor module includes appointment approval, calendar scheduling, and availability management. Implemented real-time updates using WebSockets, and integrated Google Maps API for location-based doctor searches.',
    technologies: ['Flutter', 'Node.js', 'MongoDB', 'Firebase'],
    category: 'Mobile Apps',
    confidential: true,
  },
  {
    id: 7,
    title: 'FabLab Smart Cabinet',
    description:' Development of an innovative solution combining a smart cabinet with two separate web platforms for a FabLab. The cabinet, equipped with sensors and actuators, secures the equipment and guides users with light indicators. The web platforms, one for administrators and the other for beneficiaries, simplify inventory and request management. This solution has transformed resource management, optimizing operations and strengthening security within the FabLab.',
    technologies: ['Mean Stack', 'IOT', 'PCB', 'ESP32'],
    category: 'Web Development'
  },
  {
    id: 8,
    title: 'Ecommerce Website',
    description: 'Built a full-featured ecommerce platform with product catalog, shopping cart, secure checkout, and order management. Implemented user authentication, product search/filtering, reviews, and real-time inventory tracking. Integrated payment processing with Stripe and email notifications for order updates.',
    technologies: ['Angular', 'Node.js', 'MongoDB', 'Express'],
    category: 'Web Development'
  },
  {
    id: 9,
    title: 'Social Media Website',
    description: 'Built an interactive social media platform where users can create profiles, share photos and posts, and engage with others through comments and likes. Implemented real-time notifications, direct messaging, and photo filters. Features include user authentication, profile customization, photo/video uploads with cloud storage, news feed with infinite scroll, and robust search functionality.',
    technologies: ['Angular', 'Node.js', 'MongoDB'],
    category: 'Web Development'
  },
  {
    id: 10,
    title: 'Fitness Club Website',
    description: 'Developed a responsive website for a fitness club that displays membership plans and special offers. Implemented user authentication, subscription management, and a dynamic pricing system. Users can view detailed membership options, compare plans, and sign up for memberships with secure payment processing.',
    technologies: ['Angular', 'Node.js', 'MongoDB', 'Stripe'],
    category: 'Web Development'
  },
  {
    id: 11,
    title: 'Intervention Platform',
    description: 'Developed a comprehensive intervention management platform for handling invoices, parts inventory, products, and customer claims. Features include automated invoice generation, real-time parts tracking, product catalog management, and a streamlined claims processing system. Implemented role-based access control, reporting dashboards, and email notifications for claim status updates.',
    technologies: ['Angular', 'Node.js', 'MongoDB', 'Express'],
    category: 'Web Development',
    confidential: true
  }
  

  /*
  {
    id: 8,
    title: '',
    description:'',
    technologies: ['Mean Stack', 'IOT', 'PCB', 'ESP32'],
    category: 'Web Development'
  }
    */
]

const filteredProjects = computed(() => {
  const category = selectedCategory.value
  let filtered = category === 'All' ? projects : projects.filter(project => project.category === category)
  return filtered.slice(0, visibleCount.value)
})

const hasMoreProjects = computed(() => {
  const category = selectedCategory.value
  const totalProjects = category === 'All' ? 
    projects.length : 
    projects.filter(project => project.category === category).length
  return visibleCount.value < totalProjects
})

const getCategoryCount = (category) => {
  if (category === 'All') return projects.length
  return projects.filter(project => project.category === category).length
}

const toggleShowAll = () => {
  const category = selectedCategory.value
  const totalProjects = category === 'All' ? 
    projects.length : 
    projects.filter(project => project.category === category).length
  
  if (visibleCount.value >= totalProjects) {
    visibleCount.value = 3
  } else {
    visibleCount.value = Math.min(visibleCount.value + 3, totalProjects)
  }
}

const handleCategoryChange = (category) => {
  selectedCategory.value = category
  visibleCount.value = 3
}
</script>

<style scoped>
.projects-section {
  position: relative;
  background-color: var(--v-surface-variant);
  width: 100%;
  overflow: hidden;
}

.project-parallax {
  width: 100%;
}

.section-content {
  width: 100%;
  max-width: 1800px;
  margin: 0 auto;
  padding-left: max(32px, env(safe-area-inset-left));
  padding-right: max(32px, env(safe-area-inset-right));
}

.overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, 
    rgba(16, 16, 24, 0.98) 0%,
    rgba(16, 16, 24, 0.94) 50%,
    rgba(16, 16, 24, 0.92) 100%
  );
  backdrop-filter: blur(12px);
}

.section-line {
  height: 2px;
  width: 100px;
  background: linear-gradient(90deg, 
    transparent 0%,
    var(--v-primary-base) 50%,
    transparent 100%
  );
  opacity: 0.9;
}

.expertise-card {
  text-align: center;
  padding: 0.1rem 0.05rem;
  background: linear-gradient(145deg, 
    rgba(255, 255, 255, 0.06),
    rgba(255, 255, 255, 0.02)
  );
  border-radius: 15px;
  border: 1px solid rgba(255, 255, 255, 0.08);
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  backdrop-filter: blur(12px);

  &:hover {
    transform: translateY(-8px);
    border-color: var(--v-primary-base);
    box-shadow: 0 12px 40px rgba(0, 0, 0, 0.2);
  }

  .expertise-icon {
    margin-bottom: 0.2rem;
    display: inline-flex;
    padding: 0.3rem;
    border-radius: 50%;
    background: linear-gradient(145deg,
      rgba(var(--v-theme-primary), 0.18),
      rgba(var(--v-theme-primary), 0.06)
    );
  }

  .expertise-count {
    color: white;
    margin-bottom: 0.3rem;
    font-size: 0.7rem;
  }

  .expertise-title {
    font-size: 0.675rem;
    opacity: 0.9;
    font-weight: 500;
  }
}

.projects-grid {
  margin: 0 -0.75rem;
}

.project-card {
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
  background: linear-gradient(165deg,
    rgba(255, 255, 255, 0.09),
    rgba(255, 255, 255, 0.04)
  );
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.12);
  height: 100%;
  position: relative;
  
  &::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(165deg,
      rgba(var(--v-theme-primary), 0.1),
      transparent
    );
    opacity: 0;
    transition: opacity 0.3s ease;
  }
  
  &:hover {
    transform: translateY(-6px);
    border-color: rgba(var(--v-theme-primary), 0.3);
    box-shadow: 
      0 8px 20px rgba(0, 0, 0, 0.3),
      0 0 30px rgba(var(--v-theme-primary), 0.15);
    
    &::before {
      opacity: 1;
    }
  }
}

.project-content-wrapper {
  height: 100%;
  display: flex;
  flex-direction: column;
  padding: 1.5rem !important;
  gap: 1rem;
  position: relative;
  z-index: 1;
}

.project-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-bottom: 0.5rem;
}

.project-badge {
  display: inline-flex;
  align-items: center;
  padding: 4px 12px;
  border-radius: 20px;
  background: rgba(var(--v-theme-primary), 0.15);
  font-size: 0.7rem;
  font-weight: 600;
  color: rgb(var(--v-theme-primary));
  border: 1px solid rgba(var(--v-theme-primary), 0.2);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.project-title {
  font-size: 1.1rem !important;
  line-height: 1.3;
  color: rgba(255, 255, 255, 0.95);
  font-weight: 600;
  margin-bottom: 0.75rem;
}

.project-description {
  color: rgba(255, 255, 255, 0.85);
  line-height: 1.6;
  font-size: 0.85rem;
  white-space: pre-line;
  margin-bottom: 0.5rem;
}

.tech-stack {
  margin-top: auto;
  padding-top: 0.75rem;
  border-top: 1px solid rgba(255, 255, 255, 0.08);
}

.tech-grid {
  display: grid;
  grid-template-rows: repeat(2, 20px);
  grid-auto-flow: column;
  grid-auto-columns: minmax(70px, auto);
  gap: 0.35rem;
  justify-content: start;
  overflow-x: auto;
  padding-bottom: 4px;
  
  scrollbar-width: none;
  -ms-overflow-style: none;
  &::-webkit-scrollbar {
    display: none;
  }
}

.tech-chip {
  margin: 0 !important;
  font-size: 0.65rem !important;
  font-weight: 500;
  height: 20px !important;
  min-height: 20px !important;
  background: rgba(var(--v-theme-primary), 0.1) !important;
  border: 1px solid rgba(var(--v-theme-primary), 0.15);
  padding: 0 8px !important;
  transition: all 0.2s ease;
  white-space: nowrap;
  justify-content: center;
  
  &:hover {
    transform: translateY(-1px);
    background: rgba(var(--v-theme-primary), 0.15) !important;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  }
}

/* Update confidential chip */
.v-chip.error {
  font-size: 0.65rem;
  height: 20px;
  background: rgba(var(--v-theme-error), 0.15) !important;
  color: rgb(var(--v-theme-error)) !important;
  border: 1px solid rgba(var(--v-theme-error), 0.25);
  padding: 0 10px;
  font-weight: 500;
}

/* Add a subtle animation for chips */
@keyframes chipFloat {
  0% { transform: translateY(0); }
  50% { transform: translateY(-2px); }
  100% { transform: translateY(0); }
}

.tech-chip:hover {
  animation: chipFloat 1s ease infinite;
}

/* Optimize grid layout */
.projects-grid {
  margin: 0 -0.75rem;
}

.project-item {
  padding: 0.75rem !important;
}

/* Add responsive adjustments */
@media (min-width: 960px) {
  .project-item {
    flex: 0 0 33.333%;
    max-width: 33.333%;
  }
  
  .project-content-wrapper {
    padding: 1.75rem !important;
  }
}

@media (min-width: 1400px) {
  .project-item {
    flex: 0 0 25%;
    max-width: 25%;
  }
}

/* Add smooth transitions for all interactive elements */
.project-card *,
.tech-chip,
.project-badge {
  transition: all 0.3s ease;
}

.project-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 0, 0, 0.92);
  backdrop-filter: blur(12px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2.5rem;
  opacity: 0;
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  overflow-y: auto;
  
  &.show {
    opacity: 1;
    backdrop-filter: blur(8px);
    background: linear-gradient(145deg,
      rgba(0, 0, 0, 0.95),
      rgba(0, 0, 0, 0.88)
    );
  }
}

.project-content {
  color: white;
  text-align: left;
  transform: translateY(30px) scale(0.95);
  transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
  max-width: 600px;
  margin: 0 auto;
  opacity: 0;

  .project-overlay.show & {
    transform: translateY(0) scale(1);
    opacity: 1;
  }

  h3 {
    font-size: 1.1rem !important;
    line-height: 1.3;
    color: rgba(255, 255, 255, 0.95);
    font-weight: 600;
    margin-bottom: 0.5rem;
  }

  p {
    font-size: 0.85rem;
    margin-bottom: 1.5rem;
    line-height: 1.6;
    color: rgba(255, 255, 255, 0.9);
  }
}

.action-btn {
  transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  text-transform: none;
  letter-spacing: 0.5px;
  min-width: 120px;
  padding: 0.5rem 1.5rem;
  font-weight: 500;
  position: relative;
  overflow: hidden;
  
  &::before {
    content: '';
    position: absolute;
    inset: -2px;
    background: linear-gradient(45deg, 
      transparent, 
      rgba(255, 255, 255, 0.2), 
      transparent
    );
    transform: translateX(-100%);
    transition: transform 0.6s;
  }
  
  &:hover {
    transform: translateY(-4px);
    box-shadow: 
      0 8px 20px rgba(0, 0, 0, 0.3),
      0 0 30px rgba(var(--v-theme-primary), 0.2);

    &::before {
      transform: translateX(100%);
    }
  }
}

@keyframes badgePulse {
  0% { box-shadow: 0 4px 15px rgba(var(--v-theme-primary), 0.2); }
  50% { box-shadow: 0 4px 25px rgba(var(--v-theme-primary), 0.4); }
  100% { box-shadow: 0 4px 15px rgba(var(--v-theme-primary), 0.2); }
}

@keyframes chipAppear {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.fade-up {
  opacity: 0;
  transform: translateY(40px);
  animation: fadeUp 0.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
}

@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(40px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

:deep(.v-slide-group__prev),
:deep(.v-slide-group__next) {
  background: linear-gradient(145deg,
    rgba(255, 255, 255, 0.08),
    rgba(255, 255, 255, 0.04)
  ) !important;
  backdrop-filter: blur(8px);
  border-radius: 50%;
  margin: 0 12px;
  border: 1px solid rgba(255, 255, 255, 0.08);
}

:deep(.v-slide-group__content) {
  padding: 8px;
}

.category-btn {
  background: linear-gradient(145deg,
    rgba(255, 255, 255, 0.06),
    rgba(255, 255, 255, 0.03)
  );
  border: 1px solid rgba(255, 255, 255, 0.08);
  
  &.selected {
    background: linear-gradient(145deg,
      rgba(var(--v-theme-primary), 0.3),
      rgba(var(--v-theme-primary), 0.1)
    );
    border-color: transparent;
  }

  .category-count {
    margin-left: 8px;
    opacity: 0.7;
    font-size: 0.8rem;
    background: rgba(255, 255, 255, 0.1);
    padding: 2px 8px;
    border-radius: 12px;
  }
}

/* Add smooth scrolling to the whole page */
html {
  scroll-behavior: smooth;
}

.button-wrapper {
  display: inline-block;
  position: relative;
  
  &::before {
    content: '';
    position: absolute;
    inset: -1px;
    background: linear-gradient(135deg,
      rgba(var(--v-theme-primary), 0.4),
      rgba(var(--v-theme-primary), 0.15)
    );
    border-radius: 50px;
    filter: blur(12px);
    opacity: 0;
    transition: opacity 0.5s ease;
  }
  
  &:hover::before {
    opacity: 0.4;
  }
}

.see-more-btn {
  background: linear-gradient(135deg,
    rgba(var(--v-theme-primary), 0.12) 0%,
    rgba(var(--v-theme-primary), 0.04) 100%
  );
  border: 1px solid rgba(var(--v-theme-primary), 0.15);
  transition: all 0.5s cubic-bezier(0.34, 1.56, 0.64, 1);
  position: relative;
  overflow: hidden;
  min-width: 160px;
  height: 40px !important;
  backdrop-filter: blur(10px);
  
  .btn-background {
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg,
      rgba(var(--v-theme-primary), 0.15),
      rgba(var(--v-theme-primary), 0.05)
    );
    opacity: 0;
    transition: opacity 0.3s ease;
  }
  
  &::before,
  &::after {
    content: '';
    position: absolute;
    inset: -1px;
    border-radius: inherit;
    padding: 1px;
    background: linear-gradient(135deg,
      rgba(var(--v-theme-primary), 0.4),
      rgba(var(--v-theme-primary), 0.1)
    );
    -webkit-mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
    mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
    -webkit-mask-composite: xor;
    mask-composite: exclude;
    opacity: 0.4;
    transition: opacity 0.3s ease;
  }
  
  &:hover {
    transform: translateY(-2px) scale(1.01);
    background: linear-gradient(135deg,
      rgba(var(--v-theme-primary), 0.15) 0%,
      rgba(var(--v-theme-primary), 0.06) 100%
    );
    
    .btn-background {
      opacity: 1;
    }
    
    &::before,
    &::after {
      opacity: 0.8;
    }
  }

  .btn-content {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 6px;
    position: relative;
    z-index: 1;
  }

  .icon-wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 20px;
    height: 20px;
    position: relative;
    
    &::before {
      content: '';
      position: absolute;
      inset: 0;
      background: rgba(var(--v-theme-primary), 0.12);
      border-radius: 50%;
      transform: scale(0);
      transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
    }
  }

  &:hover .icon-wrapper::before {
    transform: scale(1.2);
  }

  .toggle-icon {
    transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
    position: relative;
    z-index: 1;
  }

  &:hover .toggle-icon {
    transform: translateY(var(--toggle-direction, -3px));
    color: rgb(var(--v-theme-primary));
  }

  .btn-text {
    font-weight: 500;
    letter-spacing: 0.3px;
    font-size: 0.875rem;
    background: linear-gradient(135deg,
      rgba(255, 255, 255, 0.95),
      rgba(255, 255, 255, 0.85)
    );
    -webkit-background-clip: text;
    background-clip: text;
    transition: all 0.3s ease;
  }

  &:hover .btn-text {
    letter-spacing: 0.5px;
  }
}

@keyframes buttonGlow {
  0%, 100% {
    filter: brightness(1);
  }
  50% {
    filter: brightness(1.2);
  }
}

.show-less-btn {
  font-size: 0.8rem;
  letter-spacing: 0.3px;
  opacity: 0.7;
  transition: all 0.3s ease;
  
  &:hover {
    opacity: 1;
    transform: translateY(-2px);
  }
}
</style>