<template>
  <div class="container mx-auto px-4 py-8">
    <!-- Hero Section -->
    <section class="text-center mb-12">
      <div class="max-w-4xl mx-auto">
        <h1 class="text-4xl md:text-6xl font-bold bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent mb-6">
          Discover Your Life Balance
        </h1>
        <p class="text-xl text-gray-600 mb-8 leading-relaxed">
          LifeWheel is a comprehensive life balance assessment tool that helps you visualize and improve different areas of your life. Rate yourself across 8 key categories and receive personalized insights to create a more balanced, fulfilling life.
        </p>
        <div class="flex flex-col sm:flex-row gap-4 justify-center items-center mb-8">
          <button 
            @click="scrollToAssessment"
            class="px-8 py-4 bg-gradient-to-r from-blue-500 to-purple-500 text-white rounded-full font-semibold hover:from-blue-600 hover:to-purple-600 transition-all duration-200 hover:scale-105 shadow-lg"
          >
            Start Assessment
          </button>
          <button 
            @click="scrollToDemo"
            class="px-8 py-4 border-2 border-blue-500 text-blue-600 rounded-full font-semibold hover:bg-blue-50 transition-all duration-200"
          >
            Watch Demo
          </button>
        </div>
      </div>
    </section>

    <!-- Demo Video Section -->
    <section id="demo" class="mb-16">
      <div class="max-w-4xl mx-auto">
        <div class="glass-effect rounded-3xl p-8 text-center">
          <h2 class="text-3xl font-bold text-gray-800 mb-4">See LifeWheel in Action</h2>
          <p class="text-gray-600 mb-6">Watch how easy it is to assess and improve your life balance</p>
          
          <!-- Demo Video Placeholder -->
          <div class="aspect-video bg-gradient-to-br from-blue-100 to-purple-100 rounded-2xl flex items-center justify-center mb-4">
            <div class="text-center">
              <div class="w-20 h-20 bg-white rounded-full flex items-center justify-center mx-auto mb-4 shadow-lg">
                <svg class="w-8 h-8 text-blue-600" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M8 5v14l11-7z"/>
                </svg>
              </div>
              <p class="text-gray-600 font-medium">Demo Video Coming Soon</p>
              <p class="text-sm text-gray-500">YouTube integration placeholder</p>
            </div>
          </div>
          
          <p class="text-sm text-gray-500">
            This space is reserved for a YouTube demo video showing how to use LifeWheel effectively.
          </p>
        </div>
      </div>
    </section>

    <!-- Assessment Section -->
    <section id="assessment" class="mb-16">
      <div class="max-w-7xl mx-auto">
        <div class="text-center mb-12">
          <h2 class="text-3xl font-bold text-gray-800 mb-4">Life Balance Assessment</h2>
          <p class="text-gray-600 max-w-2xl mx-auto">
            Rate each area of your life from 1 to 10. Be honest with yourself - this assessment is for your personal growth and insight.
          </p>
        </div>

        <div class="grid lg:grid-cols-2 gap-8">
          <!-- Categories Section -->
          <div class="space-y-6">
            <div class="glass-effect rounded-2xl p-6">
              <h3 class="text-xl font-semibold text-gray-800 mb-6">Rate Your Life Areas</h3>
              <div class="space-y-6">
                <CategorySlider
                  v-for="category in categories"
                  :key="category.name"
                  :category="category"
                  :value="scores[category.name]"
                  @update:value="updateScore(category.name, $event)"
                />
              </div>
              
              <div class="mt-8 pt-6 border-t border-gray-200">
                <div class="flex justify-between items-center">
                  <div>
                    <div class="text-sm text-gray-600">Overall Score</div>
                    <div class="text-2xl font-bold" :class="overallScoreColor">
                      {{ overallScore.toFixed(1) }}/10
                    </div>
                  </div>
                  <button
                    @click="resetAssessment"
                    class="px-6 py-2 border border-gray-300 text-gray-600 rounded-full hover:bg-gray-50 transition-colors duration-200"
                  >
                    Reset All
                  </button>
                </div>
              </div>
            </div>
          </div>

          <!-- Visualization Section -->
          <div class="space-y-6">
            <!-- Chart -->
            <div class="glass-effect rounded-2xl p-6">
              <h3 class="text-xl font-semibold text-gray-800 mb-6 text-center">Your Life Wheel</h3>
              <div class="h-80">
                <LifeWheelChart :data="chartData" />
              </div>
            </div>

            <!-- Quick Stats -->
            <div class="glass-effect rounded-2xl p-6">
              <h4 class="font-semibold text-gray-800 mb-4">Quick Stats</h4>
              <div class="grid grid-cols-2 gap-4">
                <div class="text-center p-4 bg-green-50 rounded-xl">
                  <div class="text-2xl font-bold text-green-600">{{ strengthAreas }}</div>
                  <div class="text-sm text-green-600">Strong Areas</div>
                </div>
                <div class="text-center p-4 bg-yellow-50 rounded-xl">
                  <div class="text-2xl font-bold text-yellow-600">{{ improvementAreas }}</div>
                  <div class="text-sm text-yellow-600">Growth Areas</div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Feedback Section -->
    <section class="mb-16">
      <div class="max-w-4xl mx-auto">
        <Transition name="slide-up" appear>
          <FeedbackPanel :scores="scores" />
        </Transition>
      </div>
    </section>
  </div>
</template>

<script setup>
useHead({
  title: 'LifeWheel - Life Balance Assessment Tool',
  meta: [
    { name: 'description', content: 'Assess and visualize your life balance across key areas with personalized insights and recommendations. Create a more balanced, fulfilling life with LifeWheel.' }
  ]
})

const categories = [
  {
    name: 'Health',
    description: 'Physical and mental well-being'
  },
  {
    name: 'Career',
    description: 'Professional growth and satisfaction'
  },
  {
    name: 'Finances',
    description: 'Financial stability and security'
  },
  {
    name: 'Relationships',
    description: 'Family, friends, and social connections'
  },
  {
    name: 'Personal Growth',
    description: 'Learning and self-development'
  },
  {
    name: 'Leisure',
    description: 'Fun, hobbies, and relaxation'
  },
  {
    name: 'Spirituality',
    description: 'Purpose, values, and inner peace'
  },
  {
    name: 'Environment',
    description: 'Living and working spaces'
  }
]

const scores = ref({
  'Health': 5,
  'Career': 5,
  'Finances': 5,
  'Relationships': 5,
  'Personal Growth': 5,
  'Leisure': 5,
  'Spirituality': 5,
  'Environment': 5
})

const updateScore = (category, value) => {
  scores.value[category] = value
}

const resetAssessment = () => {
  Object.keys(scores.value).forEach(key => {
    scores.value[key] = 5
  })
}

const overallScore = computed(() => {
  const total = Object.values(scores.value).reduce((sum, score) => sum + score, 0)
  return total / Object.keys(scores.value).length
})

const overallScoreColor = computed(() => {
  if (overallScore.value >= 8) return 'text-green-600'
  if (overallScore.value >= 6) return 'text-yellow-600'
  return 'text-red-600'
})

const strengthAreas = computed(() => {
  return Object.values(scores.value).filter(score => score >= 8).length
})

const improvementAreas = computed(() => {
  return Object.values(scores.value).filter(score => score < 6).length
})

const chartData = computed(() => {
  return {
    labels: categories.map(cat => cat.name),
    scores: categories.map(cat => scores.value[cat.name])
  }
})

const scrollToAssessment = () => {
  document.getElementById('assessment')?.scrollIntoView({ behavior: 'smooth' })
}

const scrollToDemo = () => {
  document.getElementById('demo')?.scrollIntoView({ behavior: 'smooth' })
}
</script>