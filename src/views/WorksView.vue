<template>
  <main class="pt-32">
    <!-- Hero Section -->
    <section class="py-20 bg-gray-50">
      <div class="container mx-auto px-4 text-center">
        <h1 class="text-4xl md:text-5xl font-display font-light text-primary mb-6 animate-fade-in">
          我的作品，來自每一段用心的設計旅程
        </h1>
        <p class="text-xl text-secondary max-w-2xl mx-auto animate-fade-in" style="animation-delay: 0.2s">
          這裡收藏了我在不同領域裡，用設計記錄的每一次精彩。
        </p>
      </div>
    </section>

    <!-- Works Grid -->
    <section class="py-20">
      <div class="container mx-auto px-4">
        <!-- Filter Categories -->
        <div class="flex flex-wrap justify-center gap-3 mb-16">
          <button 
            v-for="category in categories" 
            :key="category"
            @click="activeCategory = category"
            :class="[
              'px-6 py-2 rounded-full text-sm tracking-wide transition-all duration-300',
              activeCategory === category 
                ? 'bg-primary text-white shadow-md' 
                : 'bg-gray-50 text-secondary hover:bg-gray-100 border border-gray-200'
            ]"
          >
            {{ category }}
          </button>
        </div>

        <!-- Works Grid -->
        <TransitionGroup 
          name="works-grid" 
          tag="div" 
          class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8"
        >
          <div 
            v-for="work in filteredWorks" 
            :key="work.id"
            class="group flex flex-col justify-between h-full"
          >
            <div 
              class="card bg-white rounded-xl shadow-sm hover:shadow-xl transition-all duration-500 flex flex-col justify-between h-full overflow-hidden cursor-pointer"
              @click="openPreview(work)"
            >
              <!-- Image -->
              <div class="relative w-full aspect-[4/3] overflow-hidden">
                <img 
                  :src="work.type === 'group' ? work.images[0] : work.image" 
                  :alt="work.title"
                  class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-105 group-hover:shadow-2xl"
                />
                <div class="absolute inset-0 pointer-events-none group-hover:bg-black/10 transition-colors duration-300"></div>
                
                <!-- Group indicator -->
                <div v-if="work.type === 'group'" class="absolute top-3 right-3 bg-white/90 backdrop-blur-sm rounded-full px-2 py-1 text-xs font-medium text-gray-800 shadow-lg">
                  {{ work.images.length }} 張
                </div>
                
                <!-- Click hint overlay -->
                <div class="absolute inset-0 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity duration-300">
                  <div class="bg-white/90 backdrop-blur-sm rounded-full p-3 shadow-lg">
                    <svg class="w-6 h-6 text-gray-800" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0zM10 7v3m0 0v3m0-3h3m-3 0H7" />
                    </svg>
                  </div>
                </div>
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
        </TransitionGroup>
      </div>
    </section>

         <!-- Professional Image Preview Modal -->
     <Transition
       enter-active-class="transition duration-500 ease-out"
       enter-from-class="transform opacity-0 scale-95"
       enter-to-class="transform opacity-100 scale-100"
       leave-active-class="transition duration-300 ease-in"
       leave-from-class="transform opacity-100 scale-100"
       leave-to-class="transform opacity-0 scale-95"
     >
       <div 
         v-if="selectedWork" 
         class="fixed inset-0 z-50 flex items-center justify-center bg-black/80 backdrop-blur-sm"
         @click.self="closePreview"
       >
         <!-- Close Button -->
         <button 
           @click="closePreview"
           class="close-button absolute top-6 right-6 z-50 p-3 bg-white/10 backdrop-blur-md rounded-full hover:bg-white/20 transition-all duration-300 text-white"
         >
           <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
             <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
           </svg>
         </button>

         <!-- Zoom Controls -->
         <div class="absolute top-6 left-6 z-50 flex gap-2">
           <button 
             @click="zoomIn"
             class="p-3 bg-white/10 backdrop-blur-md rounded-full hover:bg-white/20 transition-all duration-300 text-white"
             :disabled="currentZoom >= 3"
           >
             <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
               <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0zM10 7v3m0 0v3m0-3h3m-3 0H7" />
             </svg>
           </button>
           <button 
             @click="zoomOut"
             class="p-3 bg-white/10 backdrop-blur-md rounded-full hover:bg-white/20 transition-all duration-300 text-white"
             :disabled="currentZoom <= 0.5"
           >
             <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
               <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0zM10 13v3m0 0v3m0-3h3m-3 0H7" />
             </svg>
           </button>
           <button 
             @click="resetZoom"
             class="p-3 bg-white/10 backdrop-blur-md rounded-full hover:bg-white/20 transition-all duration-300 text-white"
           >
             <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
               <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 8V4m0 0h4M4 4l5 5m11-1V4m0 0h-4m4 0l-5 5M4 16v4m0 0h4m-4 0l5-5m11 5l-5-5m5 5v-4m0 4h-4" />
             </svg>
           </button>
         </div>

                             <!-- Image Container - Full Screen Focus -->
         <div 
           ref="imageContainer"
           class="relative flex items-center justify-center w-full h-full p-4"
         >
           <img 
             :src="selectedWork.type === 'group' ? selectedWork.images[currentImageIndex] : selectedWork.image" 
             :alt="selectedWork.title"
             ref="previewImage"
             class="max-w-full max-h-full object-contain rounded-lg shadow-2xl transform-gpu"
             @load="onImageLoad"
           />
           
           <!-- Loading State -->
           <div v-if="imageLoading" class="absolute inset-0 flex items-center justify-center">
             <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-white"></div>
           </div>

           <!-- Navigation for group images -->
           <div v-if="selectedWork.type === 'group' && selectedWork.images.length > 1" class="absolute inset-x-0 top-1/2 transform -translate-y-1/2 flex justify-between items-center px-4">
             <!-- Previous button -->
             <button 
               @click="previousImage"
               :disabled="currentImageIndex === 0"
               class="p-3 bg-white/10 backdrop-blur-md rounded-full hover:bg-white/20 transition-all duration-300 text-white disabled:opacity-50 disabled:cursor-not-allowed"
             >
               <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                 <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
               </svg>
             </button>
             
             <!-- Next button -->
             <button 
               @click="nextImage"
               :disabled="currentImageIndex === selectedWork.images.length - 1"
               class="p-3 bg-white/10 backdrop-blur-md rounded-full hover:bg-white/20 transition-all duration-300 text-white disabled:opacity-50 disabled:cursor-not-allowed"
             >
               <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                 <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
               </svg>
             </button>
           </div>

           <!-- Image counter for group -->
           <div v-if="selectedWork.type === 'group' && selectedWork.images.length > 1" class="absolute bottom-6 left-1/2 transform -translate-x-1/2 bg-black/30 backdrop-blur-sm px-4 py-2 rounded-full text-white text-sm">
             {{ currentImageIndex + 1 }} / {{ selectedWork.images.length }}
           </div>
         </div>

         <!-- Navigation Hints -->
         <div class="absolute bottom-6 left-1/2 transform -translate-x-1/2 text-white/40 text-sm bg-black/30 backdrop-blur-sm px-3 py-1 rounded-full">
           <p v-if="selectedWork.type === 'group'">ESC 關閉 | ← → 切換</p>
           <p v-else>ESC 關閉</p>
         </div>
       </div>
     </Transition>
  </main>
</template>

<script setup>
import { ref, computed, nextTick } from 'vue'
import { onMounted, onUnmounted } from 'vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const categories = ['All', '貼文設計', '印刷品', 'AI設計']
const activeCategory = ref('All')
const selectedWork = ref(null)
const imageContainer = ref(null)
const previewImage = ref(null)
const infoOverlay = ref(null)
const imageLoading = ref(false)
const currentZoom = ref(1)
const currentImageIndex = ref(0)

const works = [
  // 組合1 - 貼文設計系列
  {
    id: 1,
    title: 'Blush Bloom｜粉櫻綻放的氣色奇蹟',
    description: '在淡櫻紛飛的柔光中，唇色輕輕綻放。結合獨家專利技術與日系美學調色，一抹粉嫩，宛如春日初吻。告別蠟黃與暗沉，以不紅腫、短修復期，打造專屬於你的櫻花好氣色。真實實拍，證明每一份粉嫩都是值得相信的改變。',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works-com/組合1-1.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合1-2.png', import.meta.url).href
    ]
  },
  // 組合2 - 貼文設計系列
  {
    id: 2,
    title: 'Timeless Duality｜青春雙效 同步綻放',
    description: '從細胞深處綻放青春，不僅是逆齡，更是重啟肌膚與身體的雙重能量。左瓶以柔霧粉包裹溫和美白力量，淨斑提亮，煥現透光肌；右瓶則以黑金格調承載高純度 NMN，於夜間修復、喚醒細胞生命力。一日雙軌，一內一外，輕鬆走進真正「由內而外」的青春迴圈。',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works-com/組合2-1.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合2-2.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合2-3.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合2-4.png', import.meta.url).href
    ]
  },
  // 組合3 - 貼文設計系列
  {
    id: 3,
    title: 'Nude Radiance｜裸感光采的極簡奢養',
    description: '一瓶，為每一次素顏準備的高級底氣。來自法國 Raffiné 的專業級防護精華，集防曬、潤色、緊緻於一身，無須多層堆疊，即刻展現裸肌自帶濾鏡的光澤感。',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works-com/組合3-1.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合3-2.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合3-3.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合3-4.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合3-5.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合3-6.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合3-7.png', import.meta.url).href
    ]
  },
  // 組合4 - 貼文設計系列
  {
    id: 4,
    title: 'Bright Cell Rhythm｜由內而外的透白節奏',
    description: '精心策劃的視覺系列，每個元素都經過深思熟慮的設計。每一顆膠囊，都是為青春定調的音符。專利亮白補充丸，17種美膚配方調和成溫柔而精準的透亮旋律，改善黯沉、均勻膚色，讓紅潤氣色悄悄綻放；',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works-com/組合4-1.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合4-2.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合4-3.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合4-4.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合4-5.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合4-6.png', import.meta.url).href
    ]
  },
  // 組合5 - 貼文設計系列
  {
    id: 5,
    title: 'Gentle Renewal｜六步極光修復療程',
    description: '以第四代醫學級果酸科技為核心，LHA LALA 療程以溫和不傷膚的方式進行細緻更新。不僅改善肌膚紋理，更同步滋養肌底，讓你在每一次療程中，重新感受水潤、光澤、柔嫩的重生節奏。',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works-com/組合5-1.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合5-2.png', import.meta.url).href
    ]
  },
  // 組合6 - 貼文設計系列
  {
    id: 6,
    title: 'Luminance Precision｜精準透白的動能釋放',
    description: '在醫美級美白進程中，真正改寫膚色命運的，不只是療程，而是成分的科學選擇。WHITENING MESO 精準導入傳明酸，以三重作用深入肌底',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works-com/組合6-1.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合6-2.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合6-3.png', import.meta.url).href,
      new URL('../assets/images/works-com/組合6-4.png', import.meta.url).href
    ]
  },
  // 組合7 - 療癒系設計系列
  {
    id: 7,
    title: 'Silent Craft｜靜中有力的職人療癒',
    description: '在一雙溫柔而堅定的手中，藏著職人的深厚底蘊。SUNG 松 ‧ 匠心手藝，從指尖緩緩傳遞溫度，深入疲憊與壓力的縫隙之間，',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works/work-17.png', import.meta.url).href,
      new URL('../assets/images/works/work-18.png', import.meta.url).href
    ]
  },
  // 組合12 - 數位焦慮系列
  {
    id: 12,
    title: 'Digital Drain｜當滑動成為大腦的慢性疲勞',
    description: '不是你不夠努力，而是大腦正悄悄被手機掏空。過度滑手機帶來多巴胺刺激泛濫、專注力迴路退化與思緒破碎化，數位生活不是敵人，但我們需要更有意識的使用方式，讓科技為你所用，而非主宰你的專注力。',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works/work-4.png', import.meta.url).href,
      new URL('../assets/images/works/work-2.png', import.meta.url).href
    ]
  },
  // 組合8 - Pizza Time 系列
  {
    id: 8,
    title: 'Slice of Love｜一口濃情，共享媽媽的專屬時光',
    description: '這個母親節，不必出門也能把最熱騰騰的心意送到媽媽面前！Pizza Time 限定推出「母親節聯名套餐」，將澳門超人氣美味 PIZZA 直送香港，全家圍坐一桌，只為那一口熟悉的香氣與久違的陪伴。',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works/work-25.png', import.meta.url).href,
      new URL('../assets/images/works/work-26.png', import.meta.url).href,
      new URL('../assets/images/works/work-27.png', import.meta.url).href
    ]
  },
  // 組合9 - 求婚策劃系列
  {
    id: 9,
    title: 'Scene of Yes｜揭開專屬你的浪漫告白場景',
    description: '每段愛情，都值得一場剛剛好的告白。是夕陽下的沙灘？熱氣球上的天空？還是滿地玫瑰的私密驚喜？為你和他／她的命定瞬間，打造最動人的開場白。',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works/work-30.png', import.meta.url).href,
      new URL('../assets/images/works/work-31.png', import.meta.url).href,
      new URL('../assets/images/works/work-32.png', import.meta.url).href
    ]
  },
  // 組合10 - 健力痛症系列
  {
    id: 10,
    title: 'Trigger Point｜痛點背後的真相，你了解幾多？',
    description: '每日肩膀痠痛、手臂舉唔高、甚至痛到連瞓覺都翻身困難？你以為係壓力太大，其實可能係「周肩炎」暗暗作怪。健力痛症治療中心透過專業治療，從筋膜釋放、關節鬆動到姿勢調整，幫你找出根源、對症舒緩，唔使再靠止痛藥硬撐！',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works/work-33.png', import.meta.url).href,
      new URL('../assets/images/works/work-34.png', import.meta.url).href
    ]
  },
  // 組合11 - 親Party 系列
  {
    id: 11,
    title: 'Party Talk｜準備好資料，Party就無難度！',
    description: '「我想做布置呀～」這句話雖簡單，背後藏著好多選項需要確認！為咗幫你打造最完美的派對佈置，我哋準備咗「派對前小提醒」系列，跟住熊熊的指引，一齊為派對開啟完美第一步！',
    category: '貼文設計',
    type: 'group',
    images: [
      new URL('../assets/images/works/work-35.png', import.meta.url).href,
      new URL('../assets/images/works/work-36.png', import.meta.url).href,
      new URL('../assets/images/works/work-37.png', import.meta.url).href
    ]
  },
  // 單張作品
  {
    id: 12,
    title: 'A Bite of Dreams｜一口夢境',
    description: '用柔和筆觸打造療癒系視覺，讓品牌在日常中悄悄盛開。',
    category: '印刷品',
    type: 'single',
    image: new URL('../assets/images/works/work-3.png', import.meta.url).href
  },
  {
    id: 13,
    title: 'The First Bite of Love｜初嚐戀愛的滋味',
    description: '熱情番茄、醇厚起司與新鮮羅勒，在炙熱爐火中共舞。每一口，都是經典瑪格麗特的純粹告白，為日常注入一抹溫柔而鮮明的悸動。',
    category: '貼文設計',
    type: 'single',
    image: new URL('../assets/images/works/work-7.png', import.meta.url).href
  },
  {
    id: 14,
    title: 'Whispers of an Apple｜蘋果的呢喃',
    description: '一顆蘋果，藏著無數溫柔的想像。以輕盈手繪描摹自然，賦予日常物件新的詩意生命。',
    category: '印刷品',
    type: 'single',
    image: new URL('../assets/images/works/work-6.png', import.meta.url).href
  },
  {
    id: 15,
    title: 'Gravityless Touch｜無重力的輕盈瞬間',
    description: '一拍貼合，宛如無重力般細緻服貼。輕盈粉體，將完美膚觸輕輕釋放，讓每一次上妝都如羽毛般無痕、自然綻放無瑕光采。',
    category: '貼文設計',
    type: 'single',
    image: new URL('../assets/images/works/work-8.png', import.meta.url).href
  },
  {
    id: 16,
    title: 'A Day of Privilege｜專屬禮遇之日',
    description: '在流光中，為摯愛品牌而來的日子。專屬會員禮遇，讓每一次選擇都值得被珍藏，限時珍貴，成就每一段不凡體驗。',
    category: '貼文設計',
    type: 'single',
    image: new URL('../assets/images/works/work-9.png', import.meta.url).href
  },
  {
    id: 17,
    title: 'A Picnic of Light｜輕盈午後的野餐夢',
    description: '在暖陽與微風中，甜點、笑聲與慵懶的片刻交織成畫。AI 筆觸下的小貓，戴著夏日微笑，在野餐墊上恣意伸展，喚醒每個人心中最柔軟的夏天。',
    category: 'AI設計',
    type: 'single',
    image: new URL('../assets/images/works/work-10.png', import.meta.url).href
  },
  {
    id: 18,
    title: 'Neon Pulse｜霓光脈搏的少女覺醒',
    description: '在電光閃耀的世界裡，她以無畏之姿喚醒未來。霓虹與速度交織成新生代的節奏，每一個目光，都映照著無限可能的冒險啟程。',
    category: 'AI設計',
    type: 'single',
    image: new URL('../assets/images/works/work-11.jpg', import.meta.url).href
  },
  {
    id: 19,
    title: 'Festive Whispers｜節慶裡的微笑邀請',
    description: '在日系紅燈籠與暖湯香氣中，拋飛飛鏢、分享一碗關東煮，用遊戲和味覺編織出最柔軟的相遇時光。每個笑聲，都讓冬日市集更加溫暖。',
    category: '印刷品',
    type: 'single',
    image: new URL('../assets/images/works/work-12.png', import.meta.url).href
  },
  {
    id: 20,
    title: 'Retro Reverie｜時光派對的倒數節奏',
    description: '揉合復古情懷與青春律動，倒數的每一天，都是記憶裡最鮮明的片段。透過視覺節奏串連倒數、抽獎與感謝，讓社群互動在懷舊與期待間悄悄升溫。',
    category: '貼文設計',
    type: 'single',
    image: new URL('../assets/images/works/work-13.png', import.meta.url).href
  },
  {
    id: 21,
    title: 'Retro Glitch｜嬉鬧復古的小宇宙',
    description: '以粗顆粒筆觸與鮮明配色，捕捉復古派對裡的狂歡記憶。每一枚貼紙，都是一段不羈的呢喃，讓歡笑與懷舊情緒，在指尖悄悄發酵。',
    category: '印刷品',
    type: 'single',
    image: new URL('../assets/images/works/work-14.png', import.meta.url).href
  },
  {
    id: 22,
    title: 'Moments to Collect｜時光裡的微醺印記',
    description: '以懷舊插畫搭配集點互動，將復古派對的溫度延伸至每一次舉杯、每一場笑語。設計讓參與者在集點中留下專屬於當晚的微醺記憶，凝結成獨一無二的節慶篇章。',
    category: '印刷品',
    type: 'single',
    image: new URL('../assets/images/works/work-15.png', import.meta.url).href
  },
  {
    id: 23,
    title: 'Journey to Tranquility｜一場身心放鬆的靜謐旅程',
    description: '在溫暖木色與日式圓窗間，展開一場與自己對話的旅程。每一寸細節，都是對身心溫柔的擁抱，讓心靈在靜謐中悄悄舒展。',
    category: '貼文設計',
    type: 'single',
    image: new URL('../assets/images/works/work-16.png', import.meta.url).href
  },
  {
    id: 24,
    title: 'Prism Secrets｜小五星的秘密公式',
    description: '跳脫直覺陷阱，讓數學思考更有節奏感。以活潑圖解喚醒邏輯之美，將枯燥公式化為趣味冒險，為孩子打開通往數學小宇宙的大門。',
    category: '貼文設計',
    type: 'single',
    image: new URL('../assets/images/works/work-19.png', import.meta.url).href
  },
  {
    id: 25,
    title: 'Guidance for New Beginnings｜外傭面試的溫暖起點',
    description: '在輕鬆對談中，為未來共處的每一日奠定溫柔基礎。以專業與理解，協助彼此找到最適合的頻率，開啟一段相互成就的旅程。',
    category: '貼文設計',
    type: 'single',
    image: new URL('../assets/images/works/work-21.png', import.meta.url).href
  },
  {
    id: 26,
    title: 'Steps to Shine｜為夢想鋪路的特訓節奏',
    description: '精心設計的課程節奏，讓孩子們在步步踏實中找到自信的節拍。高效解題、重點突破，讓成績成為努力的最溫暖見證。',
    category: '貼文設計',
    type: 'single',
    image: new URL('../assets/images/works/work-22.png', import.meta.url).href
  },
  {
    id: 27,
    title: 'Harmony in Care｜僱傭關係的小小心意',
    description: '從理解與尊重出發，在溫暖的對話中建立信任。提前協議休息日與工作時間，讓每一段合作關係，都在心意交織中自然生長。',
    category: '貼文設計',
    type: 'single',
    image: new URL('../assets/images/works/work-24.png', import.meta.url).href
  },
  {
    id: 28,
    title: "安域僱傭｜專業配對・高效安心",
    description: "三日極速搵人，成功率高達90%。專人貼心配對，快速上工，精準無走漏。安域僱傭以專業與誠信，助你輕鬆找到最合適的姐姐，讓家中每一刻都更溫暖、更安心。",
    category: "貼文設計",
    type: 'single',
    image: new URL('../assets/images/works/work-28.png', import.meta.url).href
  },
  {
    id: 29,
    title: "安域僱傭｜外傭工作範圍須知",
    description: "姐姐服務範圍明確，按摩不屬於標準工作內容。若因照顧需求需幫忙推拿，必須事前獲得外傭同意。強迫或暗示按摩屬違法行為，請尊重雙方權益，守法共創安心僱傭關係。",
    category: "貼文設計",
    type: 'single',
    image: new URL('../assets/images/works/work-29.png', import.meta.url).href
  }
]

const filteredWorks = computed(() => {
  if (activeCategory.value === 'All') return works
  return works.filter(work => work.category === activeCategory.value)
})

// Optimize animations
onMounted(() => {
  // Only animate cards when they first appear
  gsap.utils.toArray('.card').forEach((card, i) => {
    gsap.from(card, {
      opacity: 0,
      y: 30,
      duration: 0.4,
      delay: i * 0.05,
      scrollTrigger: {
        trigger: card,
        start: 'top 80%',
        end: 'top 20%',
        toggleActions: 'play none none none'
      }
    })
  })
  
  // Add keyboard event listener
  const handleKeydown = (e) => {
    if (e.key === 'Escape' && selectedWork.value) {
      closePreview()
    }
    
    // Navigation for group images
    if (selectedWork.value && selectedWork.value.type === 'group') {
      if (e.key === 'ArrowRight') {
        nextImage()
      } else if (e.key === 'ArrowLeft') {
        previousImage()
      }
    }
  }
  
  document.addEventListener('keydown', handleKeydown)
  
  // Cleanup on unmount
  onUnmounted(() => {
    document.removeEventListener('keydown', handleKeydown)
  })
})

// Add loading state
const isLoading = ref(false)

const openPreview = (work) => {
  selectedWork.value = work
  currentImageIndex.value = 0
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
    if (previewImage.value) {
      previewImage.value.style.maxWidth = `${maxWidth}px`
      previewImage.value.style.maxHeight = `${maxHeight}px`
    }
  }
  img.src = work.image
  
  // Smooth entrance animation
  nextTick(() => {
    if (imageContainer.value) {
      gsap.fromTo(imageContainer.value,
        { opacity: 0, scale: 0.95 },
        { opacity: 1, scale: 1, duration: 0.5, ease: 'power2.out' }
      )
    }
  })
}

const onImageLoad = () => {
  imageLoading.value = false
}

const zoomIn = () => {
  if (currentZoom.value < 3) {
    currentZoom.value = Math.min(3, currentZoom.value + 0.25)
    applyZoom()
  }
}

const zoomOut = () => {
  if (currentZoom.value > 0.5) {
    currentZoom.value = Math.max(0.5, currentZoom.value - 0.25)
    applyZoom()
  }
}

const resetZoom = () => {
  currentZoom.value = 1
  applyZoom()
}

const applyZoom = () => {
  if (previewImage.value) {
    gsap.to(previewImage.value, {
      scale: currentZoom.value,
      duration: 0.3,
      ease: 'power2.out'
    })
  }
}

const nextImage = () => {
  if (selectedWork.value && selectedWork.value.type === 'group') {
    if (currentImageIndex.value < selectedWork.value.images.length - 1) {
      currentImageIndex.value++
      imageLoading.value = true
      resetZoom()
    }
  }
}

const previousImage = () => {
  if (selectedWork.value && selectedWork.value.type === 'group') {
    if (currentImageIndex.value > 0) {
      currentImageIndex.value--
      imageLoading.value = true
      resetZoom()
    }
  }
}

const closePreview = () => {
  selectedWork.value = null
  currentZoom.value = 1
  currentImageIndex.value = 0
  document.body.style.overflow = 'auto'
}
</script>

<style scoped>
.card {
  transition: all 0.3s ease;
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
}

.works-grid-enter-active,
.works-grid-leave-active {
  transition: all 0.5s ease;
}

.works-grid-enter-from,
.works-grid-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.works-grid-move {
  transition: transform 0.5s ease;
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

/* Professional image preview styles */
.image-preview-container {
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
}

/* Smooth transitions for all interactive elements */
.image-preview-container * {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Hover effects for close button */
.close-button:hover {
  transform: scale(1.1);
  background-color: rgba(255, 255, 255, 0.2);
}

/* Responsive text sizing */
@media (max-width: 640px) {
  .preview-title {
    font-size: 1.5rem;
  }
  .preview-description {
    font-size: 0.875rem;
  }
}

/* Full screen image focus */
.image-preview-container {
  display: flex;
  align-items: center;
  justify-content: center;
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