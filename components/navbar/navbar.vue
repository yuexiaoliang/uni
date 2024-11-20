<script setup>
import { ref, computed, onMounted, getCurrentInstance } from 'vue';

const props = defineProps({
  /**
   * 是否固定在顶部
   */
  fixed: {
    type: Boolean,
    default: false
  },

  /**
   * 导航栏背景色
   */
  background: {
    type: String,
    default: 'transparent'
  },

  /**
   * 状态栏背景色
   */
  statusBarBackground: {
    type: String,
    default: 'transparent'
  },

  /**
   * 胶囊栏背景色
   */
  menuBarBackground: {
    type: String,
    default: 'transparent'
  }
});

// #ifdef MP-WEIXIN
const { statusBarHeight, windowWidth } = uni.getWindowInfo();

const statusBarStyles = computed(() => ({
  height: `${statusBarHeight}px`,
  backgroundColor: props.statusBarBackground
}));

const { top, right, width, height } = uni.getMenuButtonBoundingClientRect();

const menuBarHeight = height + (top - statusBarHeight) * 2;

const menuBarStyles = computed(() => ({
  height: `${menuBarHeight}px`,
  paddingRight: `${width + (windowWidth - right) * 2}px`,
  backgroundColor: props.menuBarBackground
}));
// #endif

// #ifndef MP-WEIXIN
const statusBarStyles = computed(() => ({
  height: `${uni.getSystemInfoSync().statusBarHeight}px`,
  backgroundColor: props.statusBarBackground
}));

const menuBarStyles = {
  height: 0
};
// #endif

const styles = computed(() => {
  return {
    background: props.background
  };
});

const placeholderStyles = ref({});
onMounted(() => {
  if (!props.fixed) return;

  const instance = getCurrentInstance();
  const query = uni.createSelectorQuery().in(instance.proxy);

  query
    .select('.wx-navbar')
    .boundingClientRect((data) => {
      placeholderStyles.value = {
        height: `${data.height}px`
      };
    })
    .exec();
});
</script>

<template>
  <view>
    <view v-if="fixed" class="wx-navbar__placeholder" :style="placeholderStyles"></view>

    <view :class="['wx-navbar', { fixed }]" :style="styles">
      <view class="wx-navbar__status-bar" :style="statusBarStyles"><slot name="status-bar" /></view>

      <view v-if="!!menuBarStyles.height" class="wx-navbar__menu-bar" :style="menuBarStyles"><slot name="menu-bar" /></view>

      <slot />
    </view>
  </view>
</template>

<style lang="scss">
.wx-navbar {
  &.fixed {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 9999;
  }

  &__placeholder {
    z-index: 9999;
  }

  &__status-bar {
    height: var(--status-bar-height, 20px);
  }

  &__menu-bar {
    height: 0;
  }
}
</style>
