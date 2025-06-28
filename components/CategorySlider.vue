<template>
  <div class="space-y-4">
    <div class="flex items-center justify-between">
      <div class="flex items-center space-x-3">
        <div class="w-10 h-10 rounded-full flex items-center justify-center" :class="iconBgClass">
          <component :is="iconComponent" class="w-5 h-5 text-white" />
        </div>
        <div>
          <h3 class="font-semibold text-gray-800">{{ category.name }}</h3>
          <p class="text-sm text-gray-600">{{ category.description }}</p>
        </div>
      </div>
      <div class="text-right">
        <div class="text-2xl font-bold" :class="scoreColorClass">{{ value }}</div>
        <div class="text-xs text-gray-500">/ 10</div>
      </div>
    </div>
    
    <div class="relative">
      <input
        type="range"
        :value="value"
        @input="updateValue"
        min="1"
        max="10"
        step="1"
        class="w-full h-2 bg-gray-200 rounded-full appearance-none cursor-pointer slider-custom"
        :style="sliderStyle"
      />
      <div class="flex justify-between text-xs text-gray-400 mt-1">
        <span>Poor</span>
        <span>Excellent</span>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  category: {
    type: Object,
    required: true
  },
  value: {
    type: Number,
    required: true
  }
})

const emit = defineEmits(['update:value'])

const updateValue = (event) => {
  emit('update:value', parseInt(event.target.value))
}

const iconComponents = {
  Health: 'IconHealth',
  Career: 'IconCareer',
  Finances: 'IconFinances',
  Relationships: 'IconRelationships',
  'Personal Growth': 'IconPersonalGrowth',
  Leisure: 'IconLeisure',
  Spirituality: 'IconSpirituality',
  Environment: 'IconEnvironment'
}

const iconComponent = computed(() => {
  return resolveComponent(iconComponents[props.category.name] || 'IconDefault')
})

const iconBgClass = computed(() => {
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
  return colors[props.category.name] || 'bg-gray-500'
})

const scoreColorClass = computed(() => {
  if (props.value >= 8) return 'text-green-600'
  if (props.value >= 6) return 'text-yellow-600'
  return 'text-red-600'
})

const sliderStyle = computed(() => {
  const percentage = ((props.value - 1) / 9) * 100
  return {
    background: `linear-gradient(to right, #3B82F6 0%, #8B5CF6 ${percentage}%, #E5E7EB ${percentage}%, #E5E7EB 100%)`
  }
})
</script>

<style scoped>
.slider-custom::-webkit-slider-thumb {
  appearance: none;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: white;
  border: 2px solid #3B82F6;
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  transition: all 0.2s ease;
}

.slider-custom::-webkit-slider-thumb:hover {
  transform: scale(1.1);
  border-color: #8B5CF6;
}

.slider-custom::-moz-range-thumb {
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: white;
  border: 2px solid #3B82F6;
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}
</style>