<template>
  <main>
    <!-- Hero Section -->
    <section 
      ref="heroSection"
      class="min-h-screen flex items-center justify-center relative overflow-hidden bg-gradient-to-b from-white to-gray-50 transition-all duration-500"
    >
      <!-- Background Texture -->
      <div class="absolute inset-0 bg-[url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjAiIGhlaWdodD0iNjAiIHZpZXdCb3g9IjAgMCA2MCA2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZyBmaWxsPSJub25lIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiPjxwYXRoIGQ9Ik0zNiAzNGM0LjQxOCAwIDgtMy41ODIgOC04cy0zLjU4Mi04LTgtOC04IDMuNTgyLTggOCAzLjU4MiA4IDggOHoiIGZpbGw9IiMwMDAiIGZpbGwtb3BhY2l0eT0iLjAyIi8+PC9nPjwvc3ZnPg==')] opacity-5"></div>
      
      <!-- Abstract Shape -->
      <div class="absolute top-0 right-0 w-1/3 h-1/3 opacity-5">
        <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
          <path fill="#000000" d="M45.7,-76.4C58.8,-69.2,68.8,-55.1,75.2,-39.2C81.7,-23.3,84.7,-5.6,82.5,11.5C80.3,28.6,72.9,45.1,61.2,58.1C49.5,71.1,33.5,80.6,16.7,85.8C-0.2,91,-18,91.9,-33.5,84.9C-49,77.9,-62.3,63,-71.2,45.6C-80.1,28.2,-84.6,8.3,-81.8,-10.6C-79,-29.5,-68.9,-47.4,-54.3,-58.9C-39.7,-70.4,-20.6,-75.5,-1.3,-73.8C18,-72.2,36,-63.8,45.7,-76.4Z" transform="translate(100 100)" />
        </svg>
      </div>

      <div class="container mx-auto px-4 pt-20 text-center relative z-10">
        <!-- Role Tag -->
        <div class="animate-fade-in" style="animation-delay: 0.1s">
          <span class="inline-block text-sm tracking-widest text-gray-400 font-light uppercase mb-4">
            平面設計
          </span>
        </div>

        <h1 
          class="text-4xl md:text-6xl lg:text-7xl font-display font-light tracking-wide text-primary mb-8 animate-fade-in"
          style="animation-delay: 0.2s"
        >
        設計，讓每個想法被看見
        </h1>
        
        <p 
          class="text-xl md:text-2xl text-secondary max-w-2xl mx-auto mb-12 animate-fade-in"
          style="animation-delay: 0.4s"
        >
        這裡是我的小小作品集，
        希望能透過設計，讓每個夢想成真！
        </p>
        
        <div class="flex justify-center space-x-4 animate-fade-in" style="animation-delay: 0.8s">
          <router-link 
            to="/works" 
            class="btn-primary hover-lift hover-glow transform transition-all duration-500 hover:scale-105"
          >
            Portfolio
          </router-link>
          <router-link 
            to="/about" 
            class="btn border border-primary text-primary hover:bg-primary hover:text-white transition-all duration-500 hover:scale-105 hover-lift"
          >
            About Me
          </router-link>
        </div>
      </div>
      
      <!-- Decorative Elements -->
      <div class="absolute inset-0 -z-10">
        <div class="absolute top-1/4 left-1/4 w-64 h-64 bg-accent-gold/5 rounded-full blur-3xl"></div>
        <div class="absolute bottom-1/4 right-1/4 w-64 h-64 bg-accent-gold/5 rounded-full blur-3xl"></div>
      </div>
    </section>

    <!-- Featured Works Preview -->
    <section class="py-20 bg-light-gray">
      <div class="container mx-auto px-4">
        <h2 class="section-title text-center mb-12">Featured Works</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <div 
            v-for="work in featuredWorks" 
            :key="work.id"
            class="bg-white rounded-xl shadow-md hover:shadow-xl transition-all duration-500 flex flex-col justify-between h-full overflow-hidden group animate-fade-in cursor-pointer"
            @click="openWorkDetail(work)"
          >
            <!-- Image -->
            <div class="relative w-full aspect-[4/3] overflow-hidden">
              <img 
                :src="work.image" 
                :alt="work.title"
                class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-105 group-hover:shadow-2xl"
              />
              <div class="absolute inset-0 pointer-events-none group-hover:bg-black/10 transition-colors duration-300"></div>
            </div>
            <!-- Content -->
            <div class="flex flex-col flex-1 p-6 justify-between">
              <div>
                <h3 class="text-xl font-display font-light mb-2 text-gray-900">{{ work.title }}</h3>
                <p class="text-secondary text-sm mb-4 line-clamp-2 min-h-[48px]">{{ work.description }}</p>
              </div>
              <div class="min-h-[40px] flex items-end">
                <span class="inline-block text-sm px-3 py-1 bg-gray-50 text-gray-600 rounded-full border border-gray-200">
                  {{ work.category }}
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Work Detail Modal -->
    <Transition
      enter-active-class="transition duration-700 ease-out"
      enter-from-class="transform opacity-0 scale-95"
      enter-to-class="transform opacity-100 scale-100"
      leave-active-class="transition duration-300 ease-in"
      leave-from-class="transform opacity-100 scale-100"
      leave-to-class="transform opacity-0 scale-95"
    >
      <div 
        v-if="selectedWork" 
        class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/30 backdrop-blur-2xl"
        @click.self="closeWorkDetail"
        @keydown.esc="closeWorkDetail"
      >
        <!-- Background Particles -->
        <div class="modal-background absolute inset-0 opacity-0">
          <div class="absolute inset-0 bg-[url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjAiIGhlaWdodD0iNjAiIHZpZXdCb3g9IjAgMCA2MCA2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZyBmaWxsPSJub25lIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiPjxwYXRoIGQ9Ik0zNiAzNGM0LjQxOCAwIDgtMy41ODIgOC04cy0zLjU4Mi04LTgtOC04IDMuNTgyLTggOCAzLjU4MiA4IDggOHoiIGZpbGw9IiMwMDAiIGZpbGwtb3BhY2l0eT0iLjAyIi8+PC9nPjwvc3ZnPg==')] opacity-5 animate-float"></div>
        </div>

        <!-- Professional Image Preview Modal -->
        <div 
          ref="modalRef"
          class="relative w-full max-w-[95vw] max-h-[95vh] bg-black/80 backdrop-blur-3xl rounded-3xl shadow-2xl overflow-hidden"
        >
          <!-- Close Button -->
          <button 
            @click="closeWorkDetail"
            class="absolute top-6 right-6 z-50 p-3 bg-white/10 backdrop-blur-md rounded-full hover:bg-white/20 transition-all duration-300 text-white"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>

          <!-- Image Container with Smart Sizing -->
          <div 
            ref="imageRef"
            class="relative max-w-[95vw] max-h-[95vh] flex items-center justify-center p-4"
          >
            <img 
              :src="selectedWork.image" 
              :alt="selectedWork.title"
              class="max-w-full max-h-full object-contain rounded-lg shadow-2xl transform-gpu"
              @load="onImageLoad"
            />
            
            <!-- Loading State -->
            <div v-if="imageLoading" class="absolute inset-0 flex items-center justify-center">
              <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-white"></div>
            </div>
          </div>

          <!-- Navigation Hints -->
          <div class="absolute bottom-6 left-1/2 transform -translate-x-1/2 text-white/40 text-sm bg-black/30 backdrop-blur-sm px-3 py-1 rounded-full">
            <p>ESC 關閉</p>
          </div>
        </div>
      </div>
    </Transition>
  </main>
</template>

<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const selectedWork = ref(null)
const heroSection = ref(null)
const modalRef = ref(null)
const imageRef = ref(null)
const imageLoading = ref(false)

const featuredWorks = [
  {
    id: 1,
    title: 'MVP 設計作品 1',
    description: '精選的設計作品，展現獨特的視覺美學與創意理念。',
    category: '精選作品',
    image: new URL('../assets/images/mvp/組合3-1.png', import.meta.url).href
  },
  {
    id: 2,
    title: 'MVP 設計作品 2',
    description: '專業的視覺設計，傳達品牌價值與設計理念。',
    category: '精選作品',
    image: new URL('../assets/images/mvp/組合4-4.png', import.meta.url).href
  },
  {
    id: 3,
    title: 'MVP 設計作品 3',
    description: '創意十足的設計作品，展現設計師的專業能力。',
    category: '精選作品',
    image: new URL('../assets/images/mvp/work-25.png', import.meta.url).href
  },
]

const openWorkDetail = (work) => {
  selectedWork.value = work
  imageLoading.value = true
  document.body.style.overflow = 'hidden'
  
  // Preload image for better performance
  const img = new Image()
  img.onload = () => {
    imageLoading.value = false
    
    // Smart sizing based on image dimensions
    const aspectRatio = img.width / img.height
    const viewportWidth = window.innerWidth
    const viewportHeight = window.innerHeight
    
    // Calculate optimal display size
    let maxWidth, maxHeight
    
    if (aspectRatio > 1) {
      // Landscape images
      maxWidth = Math.min(viewportWidth * 0.9, img.width)
      maxHeight = Math.min(viewportHeight * 0.8, img.height)
    } else {
      // Portrait images (like 4:5 IG posts)
      maxWidth = Math.min(viewportWidth * 0.8, img.width)
      maxHeight = Math.min(viewportHeight * 0.9, img.height)
    }
    
    // Apply optimal sizing
    if (imageRef.value) {
      const imgElement = imageRef.value.querySelector('img')
      if (imgElement) {
        imgElement.style.maxWidth = `${maxWidth}px`
        imgElement.style.maxHeight = `${maxHeight}px`
      }
    }
  }
  img.src = work.image
  
  // Smooth entrance animation
  nextTick(() => {
    if (modalRef.value) {
      gsap.fromTo(modalRef.value,
        { opacity: 0, scale: 0.95 },
        { opacity: 1, scale: 1, duration: 0.5, ease: 'power2.out' }
      )
    }
  })
}

const onImageLoad = () => {
  imageLoading.value = false
}

const closeWorkDetail = () => {
  selectedWork.value = null
  document.body.style.overflow = 'auto'
}

// Add keyboard event listener
const handleKeydown = (e) => {
  if (e.key === 'Escape' && selectedWork.value) {
    closeWorkDetail()
  }
}

document.addEventListener('keydown', handleKeydown)

// Cleanup on unmount
onUnmounted(() => {
  document.removeEventListener('keydown', handleKeydown)
})

onMounted(() => {
  // Hero section scroll animation
  gsap.to(heroSection.value, {
    scrollTrigger: {
      trigger: heroSection.value,
      start: 'top top',
      end: 'bottom top',
      scrub: true,
    },
    scale: 0.98,
    opacity: 0.8,
    backgroundColor: 'rgba(255, 255, 255, 0.9)',
  })

  // Animate sections on scroll
  gsap.utils.toArray('section').forEach((section, i) => {
    if (i === 0) return // Skip hero section
    
    gsap.from(section, {
      opacity: 0,
      y: 30,
      duration: 1,
      scrollTrigger: {
        trigger: section,
        start: 'top 80%',
        end: 'top 20%',
        toggleActions: 'play none none reverse'
      }
    })
  })
})
</script>

<style scoped>
.card {
  transition: all 0.3s ease;
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

.animate-float {
  animation: float 10s ease-in-out infinite;
}

@keyframes float {
  0%, 100% {
    transform: translate(0, 0);
  }
  50% {
    transform: translate(10px, 10px);
  }
}

/* Loading animation */
@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.animate-spin {
  animation: spin 1s linear infinite;
}
</style> 