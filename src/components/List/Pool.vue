<template>
  <UiTableTr :to="{ name: 'pool', params: { id: pool.id } }">
    <div class="column-sm text-left hide-sm hide-md hide-lg">
      {{ _shortenAddress(pool.id) }}
    </div>
    <div>
      <Pie :tokens="pool.tokens" class="mr-3" size="34" />
    </div>
    <div class="flex-auto text-left">
      <div class="d-flex flex-wrap" style="max-width: 340px;">
        <div
          v-for="token in pool.tokens"
          :key="token.address"
          :class="token.symbol.length > 14 && 'tooltipped tooltipped-n'"
          :aria-label="token.symbol"
          class="d-flex flex-items-center mr-2"
        >
          <Icon name="bullet" size="16" :style="`color: ${token.color}`" />
          {{ _num(token.weightPercent / 100, 'percent-short') }}
          {{ _shorten(token.symbol, 14) }}
        </div>
      </div>
    </div>
    <UiNum
      :value="pool.swapFee"
      format="percent"
      class="column hide-sm hide-md"
    />
    <div v-text="_num(poolLiquidity, 'usd')" class="column" />
    <div
      v-text="_num(myLiquidity, 'usd')"
      format="currency"
      class="column hide-sm hide-md hide-lg"
    />
    <div
      v-text="_num(pool.lastSwapVolume, 'usd')"
      format="currency"
      class="column hide-sm hide-md hide-lg"
    />
  </UiTableTr>
</template>

<script>
import { getPoolLiquidity } from '@/helpers/price';

export default {
  props: ['pool'],
  computed: {
    poolLiquidity() {
      return getPoolLiquidity(this.pool, this.price.values);
    },
    myLiquidity() {
      const poolShares = this.subgraph.poolShares[this.pool.id];
      if (!this.pool.finalized || !poolShares) return 0;
      return (this.poolLiquidity / this.pool.totalShares) * poolShares;
    }
  }
};
</script>
