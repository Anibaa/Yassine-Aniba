<template>
  <v-container fluid id="contact" class="contact-section pa-0">
    <div class="contact-background"></div>
    <div class="floating-particles"></div>
    <div class="animated-shapes">
      <div class="shape shape-1"></div>
      <div class="shape shape-2"></div>
      <div class="shape shape-3"></div>
    </div>
    <v-row justify="center" align="center" class="contact-wrapper py-12">
      <v-col cols="12" class="text-center mb-8">
        <v-slide-y-transition appear>
          <div class="d-flex align-center justify-center">
            <div class="line-decoration mr-4"></div>
            <span class="text-overline text-primary mb-2 d-block font-weight-bold gradient-text">GET IN TOUCH</span>
            <div class="line-decoration ml-4"></div>
          </div>
        </v-slide-y-transition>
        <v-slide-y-transition appear>
          <h2 class="text-h4 font-weight-bold mb-3">{{ displayText }}<span class="cursor" :class="{ 'cursor-blink': cursorVisible }">|</span></h2>
        </v-slide-y-transition>
      </v-col>

      <v-col cols="12" md="10" lg="8">
        <v-hover v-slot="{ isHovering, props }">
          <v-card
            v-bind="props"
            class="contact-card"
            :class="{ 'card-hover': isHovering }"
            elevation="3"
            rounded="lg"
          >
            <v-row no-gutters>
              <v-col cols="12" md="5" class="contact-info-col pa-6">
                <div class="contact-info-wrapper">
                  <v-slide-x-transition group>
                    <div v-for="(info, index) in contactInfo" :key="info.title" class="mb-6">
                      <div class="d-flex align-center contact-info-item">
                        <v-avatar :color="info.color" size="36" class="mr-3 info-avatar" :style="{ transform: isHovering ? 'scale(1.1)' : 'scale(1)' }">
                          <v-icon color="white" size="16">{{ info.icon }}</v-icon>
                        </v-avatar>
                        <div>
                          <div class="text-subtitle-2 font-weight-medium">{{ info.title }}</div>
                          <a v-if="info.link" :href="info.link" class="text-body-2 text-medium-emphasis text-decoration-none hover-link">{{ info.value }}</a>
                          <span v-else class="text-body-2 text-medium-emphasis">{{ info.value }}</span>
                        </div>
                      </div>
                    </div>
                  </v-slide-x-transition>

                  <div class="social-links mt-6">
                    <div class="d-flex gap-2 justify-center">
                      <v-hover v-slot="{ isHovering: isSocialHovering, props: socialProps }" v-for="social in socialLinks" :key="social.icon">
                        <v-btn
                          v-bind="socialProps"
                          :href="social.link"
                          target="_blank"
                          icon="small"
                          variant="text"
                          :color="isSocialHovering ? 'primary' : undefined"
                          class="social-btn"
                          :class="{ 'social-hover': isSocialHovering }"
                        >
                          <v-icon size="20">{{ social.icon }}</v-icon>
                          <v-tooltip activator="parent" location="top" class="social-tooltip">
                            <div class="text-subtitle-2">{{ social.name }}</div>
                            <div class="text-caption">{{ social.description }}</div>
                          </v-tooltip>
                        </v-btn>
                      </v-hover>
                    </div>
                  </div>
                </div>
              </v-col>

              <v-divider vertical class="contact-divider"></v-divider>

              <v-col cols="12" md="7" class="contact-form-col pa-6">
                <v-form @submit.prevent="submitForm" v-model="formValid" ref="formRef">
                  <v-row dense>
                    <v-col cols="12" sm="6">
                      <v-text-field
                        v-model="form.name"
                        label="Name"
                        variant="outlined"
                        density="comfortable"
                        hide-details="auto"
                        class="mb-3 form-field"
                        :error-messages="errors.name"
                        @focus="clearError('name')"
                        prepend-inner-icon="mdi-account"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12" sm="6">
                      <v-text-field
                        v-model="form.email"
                        label="Email"
                        variant="outlined"
                        density="comfortable"
                        hide-details="auto"
                        class="mb-3 form-field"
                        :error-messages="errors.email"
                        @focus="clearError('email')"
                        prepend-inner-icon="mdi-email"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12">
                      <v-text-field
                        v-model="form.subject"
                        label="Subject"
                        variant="outlined"
                        density="comfortable"
                        hide-details="auto"
                        class="mb-3 form-field"
                        :error-messages="errors.subject"
                        @focus="clearError('subject')"
                        prepend-inner-icon="mdi-text"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12">
                      <v-textarea
                        v-model="form.message"
                        label="Message"
                        variant="outlined"
                        density="comfortable"
                        rows="4"
                        hide-details="auto"
                        class="mb-4 form-field"
                        :error-messages="errors.message"
                        @focus="clearError('message')"
                        prepend-inner-icon="mdi-message"
                      ></v-textarea>
                    </v-col>

                    <v-col cols="12">
                      <v-btn
                        type="submit"
                        color="primary"
                        size="large"
                        block
                        :loading="loading"
                        :disabled="!formValid"
                        class="submit-btn"
                        elevation="2"
                      >
                        <span class="btn-content">
                          Send Message
                          <v-icon end class="ml-2">{{ loading ? 'mdi-loading' : 'mdi-send' }}</v-icon>
                        </span>
                      </v-btn>
                    </v-col>
                  </v-row>
                </v-form>
              </v-col>
            </v-row>
          </v-card>
        </v-hover>
      </v-col>
    </v-row>

    <v-snackbar
      v-model="snackbar.show"
      :color="snackbar.color"
      :timeout="3000"
      location="top"
      class="custom-snackbar"
      elevation="4"
    >
      <div class="d-flex align-center">
        <v-icon :color="snackbar.color" class="mr-2">
          {{ snackbar.color === 'success' ? 'mdi-check-circle' : 'mdi-alert-circle' }}
        </v-icon>
        {{ snackbar.text }}
      </div>
    </v-snackbar>
  </v-container>
</template>

<style scoped>
.contact-section {
  background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
  min-height: 100vh;
  position: relative;
  display: flex;
  align-items: center;
  overflow: hidden;
}

.contact-background {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-image: 
    radial-gradient(circle at 15% 15%, rgba(var(--v-primary-base), 0.05) 0%, transparent 50%),
    radial-gradient(circle at 85% 85%, rgba(var(--v-primary-base), 0.05) 0%, transparent 50%);
  pointer-events: none;
}

.floating-particles {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  overflow: hidden;
  pointer-events: none;
}

.floating-particles::before,
.floating-particles::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  background-image: radial-gradient(circle at center, var(--v-primary-base) 0%, transparent 60%);
  opacity: 0.03;
  animation: float 20s linear infinite;
}

.floating-particles::after {
  animation-delay: -10s;
  animation-duration: 15s;
}

@keyframes float {
  0% { transform: translate3d(0, 0, 0) rotate(0deg); }
  100% { transform: translate3d(100px, 100px, 0) rotate(360deg); }
}

.animated-shapes {
  position: absolute;
  width: 100%;
  height: 100%;
  overflow: hidden;
  pointer-events: none;
}

.shape {
  position: absolute;
  border-radius: 50%;
  background: linear-gradient(45deg, var(--v-primary-base), var(--v-primary-lighten2));
  opacity: 0.1;
  animation: shapeFloat 20s infinite;
}

.shape-1 {
  width: 300px;
  height: 300px;
  top: -150px;
  left: -150px;
}

.shape-2 {
  width: 200px;
  height: 200px;
  bottom: -100px;
  right: -100px;
  animation-delay: -5s;
}

.shape-3 {
  width: 150px;
  height: 150px;
  top: 50%;
  right: 10%;
  animation-delay: -10s;
}

@keyframes shapeFloat {
  0% { transform: translate(0, 0) rotate(0deg); }
  50% { transform: translate(100px, 100px) rotate(180deg); }
  100% { transform: translate(0, 0) rotate(360deg); }
}

.line-decoration {
  height: 2px;
  width: 50px;
  background: linear-gradient(90deg, transparent, var(--v-primary-base), transparent);
}

.gradient-text {
  background: linear-gradient(90deg, var(--v-primary-base), var(--v-primary-lighten2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: gradientMove 3s ease infinite;
}

@keyframes gradientMove {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.contact-card {
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  transition: all 0.3s ease;
}

.card-hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.1);
}

.contact-info-item {
  transition: all 0.3s ease;
  padding: 8px;
  border-radius: 8px;
}

.contact-info-item:hover {
  background: rgba(var(--v-primary-base), 0.05);
  transform: translateX(4px);
}

.info-avatar {
  transition: all 0.3s ease;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.hover-link {
  color: inherit;
  transition: all 0.2s ease;
  position: relative;
}

.hover-link::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 1px;
  bottom: -2px;
  left: 0;
  background: var(--v-primary-base);
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.3s ease;
}

.hover-link:hover::after {
  transform: scaleX(1);
  transform-origin: left;
}

.social-btn {
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.social-hover {
  transform: translateY(-4px);
}

.social-btn::before {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle, rgba(var(--v-primary-base), 0.2), transparent);
  transform: scale(0);
  transition: transform 0.3s ease;
}

.social-btn:hover::before {
  transform: scale(2);
}

.contact-divider {
  position: relative;
}

.contact-divider::before,
.contact-divider::after {
  content: '';
  position: absolute;
  width: 6px;
  height: 6px;
  background: var(--v-primary-base);
  border-radius: 50%;
  left: -2px;
}

.contact-divider::before {
  top: 0;
}

.contact-divider::after {
  bottom: 0;
}

.form-field {
  transition: all 0.3s ease;
}

.form-field:deep(.v-field--focused) {
  transform: scale(1.01);
}

.form-field:deep(.v-field__outline) {
  transition: all 0.3s ease;
}

.form-field:hover:deep(.v-field__outline) {
  opacity: 0.8;
}

.submit-btn {
  transition: all 0.3s ease;
  overflow: hidden;
  position: relative;
}

.submit-btn::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.3), transparent);
  transform: translate(-50%, -50%);
  transition: width 0.6s ease, height 0.6s ease;
}

.submit-btn:hover::before {
  width: 300px;
  height: 300px;
}

.btn-content {
  position: relative;
  z-index: 1;
}

.custom-snackbar {
  backdrop-filter: blur(10px);
}

@media (max-width: 600px) {
  .contact-card {
    margin: 16px;
  }
  
  .text-h4 {
    font-size: 1.5rem !important;
  }

  .line-decoration {
    width: 30px;
  }

  .contact-divider {
    display: none;
  }
}
</style>

<script setup>
import { ref, reactive, onMounted, onUnmounted } from 'vue'
import emailjs from '@emailjs/browser'

const formValid = ref(false)
const loading = ref(false)
const formRef = ref(null)

const form = reactive({
  name: '',
  email: '',
  subject: '',
  message: ''
})

const errors = reactive({
  name: '',
  email: '',
  subject: '',
  message: ''
})

const snackbar = reactive({
  show: false,
  color: 'success',
  text: ''
})

// Typing effect
const headingText = "Let's Build Something Great"
const displayText = ref('')

onMounted(() => {
  let index = 0
  const typeInterval = setInterval(() => {
    if (index < headingText.length) {
      displayText.value += headingText[index]
      index++
    } else {
      clearInterval(typeInterval)
    }
  }, 100)
})

const contactInfo = [
  {
    icon: 'mdi-email',
    title: 'Email',
    value: 'anibayassinesn@gmail.com',
    link: 'mailto:anibayassinesn@gmail.com',
    action: { link: 'mailto:anibayassinesn@gmail.com', icon: 'mdi-send' },
    color: 'primary'
  },
  {
    icon: 'mdi-phone',
    title: 'Phone',
    value: '+216 90 552 146',
    link: 'tel:+216 90 552 146',
    action: { link: 'tel:+216 90 552 146', icon: 'mdi-phone' },
    color: 'success'
  },
  {
    icon: 'mdi-map-marker',
    title: 'Location',
    value: 'Tunis, TN',
    action: { link: 'https://maps.google.com/?q=Tunis,+TN', icon: 'mdi-map' },
    color: 'error'
  }
]

// Add mouse tracking for 3D effect
const mouseX = ref(0)
const mouseY = ref(0)
const cursorVisible = ref(true)

const handleMouseMove = (e) => {
  const rect = e.currentTarget.getBoundingClientRect()
  mouseX.value = ((e.clientX - rect.left) / rect.width - 0.5) * 2
  mouseY.value = ((e.clientY - rect.top) / rect.height - 0.5) * 2
}

onMounted(() => {
  document.addEventListener('mousemove', handleMouseMove)
  setInterval(() => {
    cursorVisible.value = !cursorVisible.value
  }, 500)
})

onUnmounted(() => {
  document.removeEventListener('mousemove', handleMouseMove)
})

// Update social links with descriptions
const socialLinks = [
  { 
    icon: 'mdi-github',
    link: 'https://github.com/Anibaa',
    name: 'GitHub',
    description: 'Check out my code repositories'
  },
  { 
    icon: 'mdi-linkedin',
    link: 'https://www.linkedin.com/in/yassine-aniba-63a981224/',
    name: 'LinkedIn',
    description: 'Connect with me professionally'
  }
]

const clearError = (field) => {
  errors[field] = ''
}

const validateForm = () => {
  let isValid = true
  
  if (!form.name) {
    errors.name = 'Name is required'
    isValid = false
  }
  
  if (!form.email) {
    errors.email = 'Email is required'
    isValid = false
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) {
    errors.email = 'Please enter a valid email address'
    isValid = false
  }
  
  if (!form.subject) {
    errors.subject = 'Subject is required'
    isValid = false
  }
  
  if (!form.message) {
    errors.message = 'Message is required'
    isValid = false
  }
  
  return isValid
}

const resetForm = () => {
  Object.keys(form).forEach(key => form[key] = '')
  formValid.value = false
  if (formRef.value) {
    formRef.value.reset()
  }
}

const submitForm = async () => {
  if (!validateForm()) return

  loading.value = true
  try {
    // Initialize EmailJS with your public key
    emailjs.init(import.meta.env.VITE_EMAILJS_PUBLIC_KEY)

    const templateParams = {
      from_name: form.name,
      from_email: form.email,
      subject: form.subject,
      message: form.message,
      to_name: "Yassine",
      reply_to: form.email,
      to_email: "anibayassinesn@gmail.com" // Adding your email explicitly
    }

    const response = await emailjs.send(
      import.meta.env.VITE_EMAILJS_SERVICE_ID,
      import.meta.env.VITE_EMAILJS_TEMPLATE_ID,
      templateParams
    )

    console.log('Email sent successfully:', response)
    snackbar.color = 'success'
    snackbar.text = 'Message sent successfully!'
    snackbar.show = true
    resetForm()
  } catch (error) {
    console.error('Error submitting form:', error)
    snackbar.color = 'error'
    snackbar.text = 'Failed to send message. Please try again.'
    snackbar.show = true
  } finally {
    loading.value = false
  }
}
</script> 