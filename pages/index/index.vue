<template>
  <navbar class="navbar" status-bar-background="red" menu-bar-background="blue" background="#000" fixed>
    <template #status-bar>
      <view class="status-bar">this is a StatusBar.</view>
    </template>

    <template #menu-bar>
      <view class="menu-bar">this is a MenuBar.</view>
    </template>

    <view class="header">this is a Header.</view>
  </navbar>

  <view style="padding-top: 30px; height: 1000px; text-align: center"> content </view>
</template>

<style lang="scss">
.navbar {
  color: #fff;
}

.status-bar {
  height: 100%;
}

.menu-bar {
  height: 100%;
  background-color: #555;
  text-align: right;
}

.header {
  height: 44px;
  line-height: 44px;
  text-align: center;
}
</style>
