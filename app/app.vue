<template>
  <div
    class="min-h-screen bg-gradient-to-b from-gray-900 via-black to-gray-800 flex items-center justify-center p-6">
    <!-- Signal Card -->
    <div
      class="bg-white/10 backdrop-blur-md border border-white/20 rounded-2xl w-full max-w-md p-8 shadow-lg text-white text-center">
      <h1 class="text-4xl font-extrabold mb-4 tracking-tight">AuricMind</h1>

      <!-- Signal Info -->
      <div v-if="loading" class="text-gray-400 mb-4">
        Fetching latest signal...
      </div>

      <div v-else-if="signal" class="space-y-4">
        <p class="text-lg font-semibold">
          Pair: <span class="text-teal-400">{{ signal.symbol }}</span>
        </p>

        <p class="text-2xl font-bold" :class="signalClass">
          Signal: {{ signal.signal }}
        </p>

        <p class="text-gray-300 text-sm leading-relaxed">
          Why this signal matters: {{ signal.reason }}
        </p>

        <div
          class="flex flex-col sm:flex-row justify-between mt-4 text-sm gap-2">
          <div class="flex flex-col items-center">
            <span class="text-gray-400">Entry Price</span>
            <span class="font-semibold text-blue-400">
              {{ signal.entryMin.toFixed(2) }} -
              {{ signal.entryMax.toFixed(2) }}
            </span>
          </div>

          <div class="flex flex-col items-center">
            <span class="text-gray-400">Stop Loss</span>
            <span class="font-semibold text-red-400">{{
              signal.stop_loss.toFixed(2)
            }}</span>
          </div>

          <div class="flex flex-col items-center">
            <span class="text-gray-400">Take Profit</span>
            <span class="font-semibold text-green-400">{{
              signal.take_profit.toFixed(2)
            }}</span>
          </div>
        </div>

        <p class="text-gray-400 text-xs mt-2">
          Last updated: {{ lastUpdated }}
        </p>
      </div>

      <div v-else class="text-gray-400 mb-4">
        Click "Refresh" to get the latest signal
      </div>

      <!-- Call-to-Action -->
      <p class="text-gray-300 mt-6 text-sm leading-relaxed">
        Maximize your trading advantage with AI insights — never miss an
        opportunity.
      </p>

      <!-- Refresh Button -->
      <div class="mt-6 flex justify-center">
        <button
          @click="fetchSignal"
          :disabled="loading"
          class="bg-teal-500 hover:bg-teal-400 transition px-6 py-2 rounded-full font-semibold flex items-center justify-center gap-2 disabled:opacity-50">
          <span
            v-if="loading"
            class="animate-spin border-2 border-white border-t-transparent rounded-full w-4 h-4"></span>
          Refresh
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";

export default {
  setup() {
    const signal = ref(null);
    const loading = ref(false);
    const lastUpdated = ref(null);

    const fetchSignal = async () => {
      loading.value = true;
      signal.value = null;
      try {
        const res = await fetch("http://localhost:3001/api/trading-signal");
        if (!res.ok) throw new Error("Failed to fetch");
        signal.value = await res.json();
        lastUpdated.value = new Date().toLocaleTimeString();
      } catch (e) {
        console.error(e);
        signal.value = null;
      } finally {
        loading.value = false;
      }
    };

    const signalClass = computed(() => {
      if (!signal.value) return "";
      return signal.value.signal === "BUY"
        ? "text-green-400"
        : signal.value.signal === "SELL"
        ? "text-red-400"
        : "text-yellow-300";
    });

    return {
      signal,
      loading,
      lastUpdated,
      fetchSignal,
      signalClass,
    };
  },
};
</script>

<style scoped>
button:disabled {
  cursor: not-allowed;
}
</style>
