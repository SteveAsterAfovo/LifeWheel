<template>
  <div class="min-h-screen flex flex-col">
    <!-- Header -->
    <header class="glass-effect sticky top-0 z-50 shadow-lg">
      <div class="container mx-auto px-4 py-4">
        <div class="flex items-center justify-between">
          <div class="flex items-center space-x-3">
            <div class="w-10 h-10 bg-gradient-to-br from-blue-500 to-purple-600 rounded-full flex items-center justify-center">
              <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
              </svg>
            </div>
            <h1 class="text-2xl font-bold bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">
              LifeWheel
            </h1>
          </div>
          <nav class="hidden md:flex space-x-6">
            <a href="#assessment" class="text-gray-600 hover:text-blue-600 transition-colors duration-200">Assessment</a>
            <a href="#demo" class="text-gray-600 hover:text-blue-600 transition-colors duration-200">Demo</a>
          </nav>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="flex-1">
      <slot />
    </main>

    <!-- Footer -->
    <footer class="glass-effect mt-auto">
      <div class="container mx-auto px-4 py-6">
        <div class="text-center space-y-4">
          <div class="flex justify-center items-center space-x-4">
            <a 
              href="https://bolt.new" 
              target="_blank" 
              rel="noopener noreferrer"
              class="inline-flex items-center space-x-2 px-4 py-2 bg-gradient-to-r from-blue-500 to-purple-500 text-white rounded-full hover:from-blue-600 hover:to-purple-600 transition-all duration-200 hover:scale-105 shadow-lg"
            >
              <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
                <path d="M13 10V3L4 14h7v7l9-11h-7z"/>
              </svg>
              <span class="text-sm font-medium">Built on Bolt</span>
            </a>
          </div>
          <p class="text-gray-600 text-sm">
            © 2025 LifeWheel. Assess and improve your life balance with personalized insights.
          </p>
        </div>
      </div>
    </footer>
  </div>
</template>