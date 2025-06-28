<template>
  <div class="space-y-6">
    <div class="text-center">
      <h3 class="text-2xl font-bold text-gray-800 mb-2">Your Personalized Insights</h3>
      <p class="text-gray-600">Based on your life balance assessment</p>
    </div>

    <div v-if="improvementAreas.length === 0" class="text-center p-8 glass-effect rounded-2xl">
      <div class="w-16 h-16 bg-green-100 rounded-full flex items-center justify-center mx-auto mb-4">
        <svg class="w-8 h-8 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
        </svg>
      </div>
      <h4 class="text-xl font-semibold text-green-800 mb-2">Excellent Balance!</h4>
      <p class="text-green-600">Your life appears to be well-balanced across all areas. Keep up the great work!</p>
    </div>

    <div v-else class="space-y-4">
      <div 
        v-for="(area, index) in improvementAreas" 
        :key="area.category"
        class="glass-effect rounded-2xl p-6 hover:shadow-lg transition-all duration-300"
        :class="{ 'animate-pulse': index === 0 }"
      >
        <div class="flex items-start space-x-4">
          <div class="w-12 h-12 rounded-full flex items-center justify-center flex-shrink-0" :class="getIconBgClass(area.category)">
            <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
            </svg>
          </div>
          <div class="flex-1">
            <h4 class="font-semibold text-gray-800 mb-2">{{ area.category }}</h4>
            <p class="text-gray-600 text-sm leading-relaxed">{{ area.advice }}</p>
            <div class="mt-3 flex items-center space-x-2">
              <span class="text-xs bg-yellow-100 text-yellow-800 px-2 py-1 rounded-full">
                Current Score: {{ area.score }}/10
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="glass-effect rounded-2xl p-6 bg-gradient-to-r from-blue-50 to-purple-50">
      <div class="flex items-start space-x-4">
        <div class="w-12 h-12 bg-gradient-to-br from-blue-500 to-purple-500 rounded-full flex items-center justify-center flex-shrink-0">
          <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z"></path>
          </svg>
        </div>
        <div class="flex-1">
          <h4 class="font-semibold text-gray-800 mb-2">💡 Pro Tip</h4>
          <p class="text-gray-600 text-sm">
            Focus on improving one area at a time. Small, consistent changes often lead to the most sustainable improvements in life balance.
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  scores: {
    type: Object,
    required: true
  }
})

const adviceDatabase = {
  Health: [
    "Consider incorporating 30 minutes of daily exercise into your routine. Even a brisk walk can make a significant difference.",
    "Focus on getting 7-8 hours of quality sleep each night. Good rest is fundamental to overall health.",
    "Try meal prepping on weekends to ensure you're eating nutritious meals throughout the week.",
    "Consider adding meditation or deep breathing exercises to your daily routine for mental wellness."
  ],
  Career: [
    "Set clear, achievable goals for your professional development. Consider what skills you'd like to develop.",
    "Network within your industry. Building relationships can open new opportunities and provide valuable insights.",
    "Seek feedback from colleagues or supervisors to identify areas for improvement and growth.",
    "Consider taking on new challenges or projects that align with your career aspirations."
  ],
  Finances: [
    "Create a monthly budget to track your income and expenses. Understanding your money flow is the first step.",
    "Start an emergency fund, even if it's just $25 per month. Small amounts add up over time.",
    "Review and optimize your recurring subscriptions and expenses. Cancel what you don't actively use.",
    "Consider learning about basic investing or speak with a financial advisor about your goals."
  ],
  Relationships: [
    "Schedule regular quality time with loved ones. Put away devices and focus on meaningful conversations.",
    "Practice active listening. Show genuine interest in what others are sharing with you.",
    "Express gratitude to people who matter to you. A simple 'thank you' can strengthen relationships.",
    "Work on resolving any ongoing conflicts through open, honest communication."
  ],
  'Personal Growth': [
    "Set aside time for learning something new, whether it's reading, taking a course, or developing a skill.",
    "Practice self-reflection through journaling or meditation to better understand your thoughts and feelings.",
    "Step out of your comfort zone regularly. Try new experiences that challenge and inspire you.",
    "Set personal goals and track your progress. Celebrate small wins along your journey."
  ],
  Leisure: [
    "Schedule regular time for activities you enjoy. Make leisure a priority, not just something that happens by accident.",
    "Try a new hobby or revisit an old one. Engaging in creative activities can be deeply fulfilling.",
    "Plan regular breaks from work and technology. Sometimes the best leisure is simply doing nothing.",
    "Spend more time in nature. Fresh air and natural surroundings can be incredibly rejuvenating."
  ],
  Spirituality: [
    "Explore practices that connect you with something greater than yourself, whether through religion, meditation, or philosophy.",
    "Spend time in quiet reflection or prayer, focusing on gratitude and inner peace.",
    "Consider volunteering for causes you care about. Helping others can provide deep spiritual fulfillment.",
    "Connect with a community that shares your values and beliefs for support and growth."
  ],
  Environment: [
    "Declutter and organize your living space. A clean environment can improve mental clarity and peace.",
    "Add plants or natural elements to your home or workspace. Nature can reduce stress and improve mood.",
    "Consider your environmental impact. Small changes like recycling or reducing waste can be meaningful.",
    "Create spaces that inspire and energize you. Your environment should support your goals and well-being."
  ]
}

const improvementAreas = computed(() => {
  const areas = []
  
  Object.entries(props.scores).forEach(([category, score]) => {
    if (score < 6) {
      const advice = adviceDatabase[category]
      const randomAdvice = advice[Math.floor(Math.random() * advice.length)]
      
      areas.push({
        category,
        score,
        advice: randomAdvice
      })
    }
  })
  
  return areas.sort((a, b) => a.score - b.score)
})

const getIconBgClass = (category) => {
  const colors = {
    Health: 'bg-red-500',
    Career: 'bg-blue-500',
    Finances: 'bg-green-500',
    Relationships: 'bg-pink-500',
    'Personal Growth': 'bg-purple-500',
    Leisure: 'bg-orange-500',
    Spirituality: 'bg-indigo-500',
    Environment: 'bg-teal-500'
  }
  return colors[category] || 'bg-gray-500'
}
</script>