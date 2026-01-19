<template>
  <div class="min-h-screen bg-[#E5E5E5] text-black font-mono p-4 md:p-8">
    <div
      class="w-full mx-auto border border-black/10 bg-white shadow-sm relative overflow-hidden">
      <header
        class="flex justify-between items-center p-6 border-b border-black/10">
        <div class="flex items-center gap-2">
          <div
            class="w-6 h-6 border-2 border-black rounded-full flex items-center justify-center">
            <div class="w-1 h-1 bg-black rounded-full"></div>
          </div>
          <span class="text-xl font-bold tracking-tighter italic">AURIC</span>
        </div>

        <nav
          class="hidden md:flex gap-8 text-[10px] uppercase font-bold tracking-widest text-gray-500">
          <a href="#" class="hover:text-black transition-colors">Dashboard</a>
          <a href="#" class="text-black border-b border-black pb-1"
            >Live Feed</a
          >
          <a href="#" class="hover:text-black transition-colors">Signals</a>
          <a href="#" class="hover:text-black transition-colors">History</a>
        </nav>

        <div class="flex gap-4 items-center">
          <span class="text-xs font-bold">JAN 19, 2026</span>
          <div
            class="w-8 h-8 bg-gray-200 rounded-sm border border-black/10"></div>
        </div>
      </header>

      <main class="p-6 md:px-10">
        <div class="grid grid-cols-12 gap-8">
          <div class="col-span-12 lg:col-span-3 space-y-12">
            <section>
              <h1 class="text-5xl font-light tracking-tighter mb-2">
                Trading Hub
              </h1>
              <p
                class="text-xs text-gray-500 leading-relaxed uppercase tracking-wider">
                Live smarter with Auric.<br />Monitor, optimize, and thrive.
              </p>
            </section>

            <div class="space-y-8">
              <div class="border-t border-black/10 pt-4 relative">
                <span class="absolute -top-1 right-0 text-[10px] opacity-30"
                  >↗</span
                >
                <h3 class="text-[10px] uppercase font-bold text-gray-400 mb-4">
                  Total Earning
                </h3>
                <div class="text-2xl font-medium tracking-tight">
                  $34,578.00 <span class="text-[10px] text-gray-400">USD</span>
                </div>
              </div>
              <div class="border-t border-black/10 pt-4 relative">
                <span class="absolute -top-1 right-0 text-[10px] opacity-30"
                  >↗</span
                >
                <h3 class="text-[10px] uppercase font-bold text-gray-400 mb-4">
                  Symbol
                </h3>
                <div class="text-2xl font-medium tracking-tight">
                  {{ signal ? signal.symbol : "0" }}
                </div>
              </div>
            </div>
          </div>

          <div
            class="col-span-12 lg:col-span-6 flex flex-col items-center justify-center py-10">
            <div
              class="relative w-full aspect-square max-w-md bg-gray-50 border border-black/5 flex items-center justify-center group">
              <div class="absolute top-2 left-2 text-[8px] opacity-20">+ +</div>
              <div class="absolute bottom-2 right-2 text-[8px] opacity-20">
                + +
              </div>

              <div class="text-center">
                <div
                  class="text-[120px] leading-none select-none opacity-10 font-bold italic">
                  AURIC
                </div>
                <button
                  @click="fetchSignal"
                  :disabled="loading"
                  class="mt-4 px-8 py-3 bg-black text-white text-xs uppercase font-bold tracking-widest hover:bg-gray-800 transition-all disabled:opacity-30">
                  {{ loading ? "Analyzing..." : "Request New Signal" }}
                </button>
              </div>
            </div>
          </div>

          <div class="col-span-12 lg:col-span-3 space-y-8 pl-4">
            <div class="border-t border-black/10 pt-4 relative">
              <span class="absolute -top-1 right-0 text-[10px] opacity-30"
                >↗</span
              >
              <h3 class="text-[10px] uppercase font-bold text-gray-400 mb-4">
                Market Trend Status
              </h3>
              <div class="flex items-end justify-between">
                {{ signal ? signal.bias : "N/A" }}
              </div>
            </div>

            <div class="border-t border-black/10 pt-4 relative">
              <span class="absolute -top-1 right-0 text-[10px] opacity-30"
                >↗</span
              >
              <h3 class="text-[10px] uppercase font-bold text-gray-400 mb-4">
                Signal Confidence
              </h3>
              <div class="relative h-20 flex items-center justify-center">
                <div
                  class="absolute inset-0 border-[10px] border-gray-100 rounded-full"></div>
                <div class="z-10 text-xl font-bold">
                  {{ signal ? signal.confidence : "0" }}%
                </div>
              </div>
            </div>
          </div>
        </div>

        <div
          class="grid grid-cols-1 md:grid-cols-4 gap-0 border-t border-black/10 mt-10 md:mt-0">
          <div class="p-6 md:border-r border-black/10">
            <h3 class="text-[10px] uppercase font-bold text-gray-400 mb-6">
              Position Details
            </h3>
            <div class="text-3xl font-light tracking-tighter mb-2">
              {{ signal?.signal || "N/A" }}
            </div>
            <p class="text-[10px] text-gray-400">
              {{ signal?.symbol || "Select Pair" }}
            </p>
          </div>

          <div class="p-6 md:border-r border-black/10">
            <h3 class="text-[10px] uppercase font-bold text-gray-400 mb-6">
              Entry Zone
            </h3>
            <div class="text-xl font-medium tracking-tight">
              {{ signal ? `${signal.entryMin} - ${signal.entryMax}` : "0.00" }}
            </div>
            <div class="flex gap-2 mt-4">
              <div
                v-for="i in 12"
                :key="i"
                :class="['w-1 h-4', i < 8 ? 'bg-black' : 'bg-gray-100']"></div>
            </div>
            <div class="text-md font-medium tracking-tight uppercase mt-4">
              INSTANT:
              {{ signal ? signal.instant : "0.00" }}
            </div>
          </div>

          <div class="p-6 md:border-r border-black/10">
            <h3 class="text-[10px] uppercase font-bold text-gray-400 mb-4">
              Risk Management
            </h3>
            <div class="space-y-2">
              <div class="flex justify-between text-[11px]">
                <span class="text-gray-400">CURRENT PRICE</span>
                <span class="text-blue-400 font-bold">{{
                  signal?.currentPrice || "0.00"
                }}</span>
              </div>
              <div class="flex justify-between text-[11px]">
                <span class="text-gray-400">STOP LOSS</span>
                <span class="text-red-400 font-bold">{{
                  signal?.stop_loss || "0.00"
                }}</span>
              </div>
              <div class="flex justify-between text-[11px]">
                <span class="text-gray-400">TAKE PROFIT</span>
                <span class="font-bold text-emerald-600">{{
                  signal?.take_profit || "0.00"
                }}</span>
              </div>
              <div class="flex justify-between text-[11px]">
                <span class="text-gray-400">R/R RATIO</span>
                <span class="font-bold">{{
                  signal?.riskReward || "0.00"
                }}</span>
              </div>
            </div>
          </div>

          <div class="p-6">
            <h3 class="text-[10px] uppercase font-bold text-gray-400 mb-6">
              System Health
            </h3>
            <div class="flex items-center gap-4">
              <div class="w-12 h-6 bg-black rounded-full p-1">
                <div class="w-4 h-4 bg-white rounded-full translate-x-6"></div>
              </div>
              <span class="text-[10px] font-bold">LIVE FEED ACTIVE</span>
            </div>
            <p class="text-[10px] text-gray-400 mt-4 uppercase">
              Last Updated: {{ lastUpdated || "Never" }}
            </p>
          </div>
        </div>
      </main>

      <div
        class="absolute top-20 bottom-0 left-[25%] w-px h-[554px] bg-black/5 pointer-events-none hidden lg:block"></div>
      <div
        class="absolute top-20 bottom-0 left-[75%] w-px h-[554px] bg-black/5 pointer-events-none hidden lg:block"></div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const signal = ref(null);
const loading = ref(false);
const lastUpdated = ref(null);

const fetchSignal = async () => {
  loading.value = true;
  // Simulated delay to show loading state
  setTimeout(async () => {
    try {
      // Logic for fetch remains the same as your original
      const res = await fetch("http://localhost:3001/api/trading-signal");
      if (!res.ok) throw new Error("Failed");
      signal.value = await res.json();
      lastUpdated.value = new Date().toLocaleTimeString();
    } catch (err) {
      console.error(err);
    } finally {
      loading.value = false;
    }
  }, 800);
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&display=swap");

body {
  font-family: "Space Mono", monospace;
  -webkit-font-smoothing: antialiased;
}
</style>
