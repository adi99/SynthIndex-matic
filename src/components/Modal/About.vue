<template>
  <UiModal :open="open" @close="$emit('close')" style="max-width: 440px;">
    <h3 class="m-4 mb-0 text-center">About</h3>
    <div class="m-4 mb-0 p-4 border rounded-2 text-white">
      <div class="d-flex">
        <span v-text="$t('version')" class="flex-auto text-gray mr-1" />
        {{ pkg.version
        }}<span v-if="commitSha" v-text="`#${commitSha.slice(0, 7)}`" />
      </div>
      <div class="d-flex">
        <span v-text="$t('license')" class="flex-auto text-gray mr-1" />
        {{ pkg.license }}
      </div>
      <div class="d-flex">
        <span v-text="$t('network')" class="flex-auto text-gray mr-1" />
        {{ config.network === 'homestead' ? 'mainnet' : config.network }}
      </div>
      <div class="d-flex">
        <span v-text="$t('blockNumber')" class="flex-auto text-gray mr-1" />
        {{ _num(web3.blockNumber, 'long') }}
      </div>
    </div>
    <div class="m-4">
      <a
        :href="`https://github.com/${pkg.repository}`"
        target="_blank"
        class="mb-2 d-block" >  
      </a>
    </div>
  </UiModal>
</template>

<script>
import pkg from '@/../package.json';

const commitSha = process.env.VUE_APP_COMMIT_SHA;

export default {
  props: ['open'],
  data() {
    return {
      pkg,
      commitSha
    };
  }
};
</script>
