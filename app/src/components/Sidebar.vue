<template>
  <aside
    class="min-h-screen p-4 space-y-6 bg-gray-800 bg-opacity-40 backdrop-filter backdrop-blur-lg rounded-lg shadow-lg"
  >
    <button
      @click="closeSidebar"
      class="absolute top-0 right-0 m-4 p-2 bg-gray-700 hover:bg-gray-600 text-white text-xs font-semibold py-2 px-4 rounded-full"
    >
      X
    </button>
    <div class="space-y-2">
      <label for="currency" class="block text-gray-300">Currency</label>
      <select
        id="currency"
        class="w-full p-2 bg-gray-700 text-white rounded-md"
        v-model="currency"
        @change="selectCurrency"
      >
        <option>Bitcoin</option>
        <option>Ethereum</option>
      </select>
    </div>

    <div class="space-y-2">
      <label class="block text-gray-300">Minimum Premium Size</label>
      <div class="flex flex-wrap gap-2">
        <button
          v-for="label in premiumSizes[currency]"
          :key="label.value"
          @click="selectMinPremSize(label.value)"
          :class="{
            'bg-purple-500 bg-opacity-40 hover:bg-opacity-80':
              minPremSize === label.value,
            'bg-gray-700 hover:bg-gray-600': minPremSize !== label.value,
          }"
          class="transition duration-150 ease-in-out p-2 flex-1 text-white font-semibold py-2 px-4 rounded shadow"
        >
          {{ label.label }}
        </button>
      </div>
    </div>

    <div class="space-y-2">
      <label class="block text-gray-300">Expiry</label>
      <div class="flex flex-wrap gap-2">
        <button
          v-for="label in expiries"
          :key="label.value"
          @click="selectExpiry(label.value)"
          :class="{
            'bg-purple-500 bg-opacity-40 hover:bg-opacity-80':
              expiry === label.value,
            'bg-gray-700 hover:bg-gray-600': expiry !== label.value,
          }"
          class="transition duration-150 ease-in-out p-2 flex-1 text-white font-semibold py-2 px-4 rounded shadow"
        >
          {{ label.label }}
        </button>
      </div>
    </div>

    <div class="space-y-2">
      <label for="unit-size" class="block text-gray-300"
        >Minimum Unit Size</label
      >
      <input
        id="unit-size"
        type="text"
        v-model="minUnitSize"
        class="w-full p-2 bg-gray-700 text-white rounded-md"
      />
    </div>

    <div class="space-y-2">
      <label class="block text-gray-300">Call or Put</label>
      <div class="flex flex-wrap gap-2">
        <button
          v-for="item in typesOption"
          :key="item.value"
          @click="selectTypeOption(item.value)"
          :class="{
            'bg-purple-500 bg-opacity-40 hover:bg-opacity-80':
              typeOption === item.value,
            'bg-gray-700 hover:bg-gray-600': typeOption !== item.value,
          }"
          class="transition duration-150 ease-in-out p-2 flex-1 text-white font-semibold py-2 px-4 rounded shadow"
        >
          {{ item.label }}
        </button>
      </div>
    </div>

    <div class="space-y-2">
      <label class="block text-gray-300">Buy or Sell</label>
      <div class="flex flex-wrap gap-2">
        <button
          v-for="item in dirsOption"
          :key="item.value"
          @click="selectDirOption(item.value)"
          :class="{
            'bg-purple-500 bg-opacity-40 hover:bg-opacity-80':
              dirOption === item.value,
            'bg-gray-700 hover:bg-gray-600': dirOption !== item.value,
          }"
          class="transition duration-150 ease-in-out p-2 flex-1 text-white font-semibold py-2 px-4 rounded shadow"
        >
          {{ item.label }}
        </button>
      </div>
    </div>

    <div class="flex gap-2">
      <button
        class="p-2 flex-1 bg-gray-600 bg-opacity-50 hover:bg-opacity-70 text-white font-semibold py-2 px-4 rounded shadow"
        @click="clearFilters"
      >
        Clear
      </button>
      <button
        class="p-2 flex-1 bg-blue-500 bg-opacity-40 hover:bg-opacity-60 text-white font-semibold py-2 px-4 rounded shadow"
        @click="emitFilters"
      >
        Go
      </button>
    </div>
  </aside>
</template>

<script>
export default {
  data() {
    return {
      currency: "Bitcoin",
      minPremSize: null,
      expiry: null,
      typeOption: null,
      dirOption: null,
      minUnitSize: null,
      premiumSizes: {
        Bitcoin: [
          { label: "0.1 BTC", value: 0.1 },
          { label: "0.5 BTC", value: 0.5 },
          { label: "1 BTC", value: 1 },
          { label: "5 BTC", value: 5 },
          { label: "10 BTC", value: 10 },
          { label: "25 BTC", value: 25 },
        ],
        Ethereum: [
          { label: "1 ETH", value: 1 },
          { label: "5 ETH", value: 5 },
          { label: "10 ETH", value: 10 },
          { label: "25 ETH", value: 25 },
          { label: "50 ETH", value: 50 },
          { label: "100 ETH", value: 100 },
        ],
      },
      expiries: [
        { label: "week", value: "WEEK" },
        { label: "month", value: "MONTH" },
        { label: "quarter", value: "QUARTER" },
        { label: "half year", value: "HALF_YEAR" },
        { label: "year", value: "YEAR" },
      ],
      typesOption: [
        { label: "PUT", value: "PUT" },
        { label: "CALL", value: "CALL" },
      ],
      dirsOption: [
        { label: "SELL", value: "SELL" },
        { label: "BUY", value: "BUY" },
      ],
    };
  },
  methods: {
    selectMinPremSize(value) {
      this.minPremSize = value;
    },
    selectExpiry(value) {
      this.expiry = value;
    },
    selectTypeOption(value) {
      this.typeOption = value;
    },
    selectDirOption(value) {
      this.dirOption = value;
    },
    clearFilters() {
      this.minPremSize = null;
      this.typeOption = null;
      this.dirOption = null;
      this.minUnitSize = null;
      this.expiry = null;
    },
    closeSidebar() {
      this.$emit("closeSidebar");
    },
    emitFilters() {
      this.$emit("filtersSelected", {
        currency: this.currency,
        minPremSize: this.minPremSize,
        minUnitSize: this.minUnitSize,
        typeOption: this.typeOption,
        dirOption: this.dirOption,
        expiry: this.expiry,
      });
      this.closeSidebar();
    },
  },
};
</script>
