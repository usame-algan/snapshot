<script setup lang="ts">
import { SNAPSHOT_HELP_LINK } from '@/helpers/constants';
import { useStorage } from '@vueuse/core';

defineProps<{
  spaceId: string;
}>();

const { web3Account } = useWeb3();

// Reactive local storage with help from vueuse package
const createdSpaces = useStorage(
  `snapshot.createdSpaces.${web3Account.value.slice(0, 8).toLowerCase()}`,
  {}
);
</script>

<template>
  <BaseBlock
    v-if="createdSpaces?.[spaceId]?.showMessage"
    class="absolute left-0 z-10 !bg-skin-bg"
  >
    <div>
      <div>
        <div>
          <h3 class="mt-0">{{ $t('newSpaceNotice.header') }}</h3>
          <div class="text-skin-text">
            <BaseIcon name="info" size="24" class="float-left mr-1" />
            <i18n-t keypath="newSpaceNotice.mainText" tag="p" scope="global">
              <template #settings>
                <BaseLink
                  :link="{ name: 'spaceSettings', params: { key: spaceId } }"
                >
                  settings</BaseLink
                >
              </template>
            </i18n-t>
          </div>

          <i18n-t
            keypath="newSpaceNotice.learnMore"
            tag="p"
            class="mt-2 text-skin-text"
            scope="global"
          >
            <template #documentation>
              <BaseLink
                link="https://docs.snapshot.org/strategies/what-is-a-strategy"
              >
                documentation</BaseLink
              >
            </template>
            <template #help>
              <BaseLink :link="SNAPSHOT_HELP_LINK">Help Center</BaseLink>
            </template>
          </i18n-t>
        </div>
        <TuneButton
          class="mt-3"
          @click="createdSpaces[spaceId].showMessage = false"
        >
          {{ $t('newSpaceNotice.gotIt') }}
        </TuneButton>
      </div>
    </div>
  </BaseBlock>
</template>
