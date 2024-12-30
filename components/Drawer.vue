<template>
  <div class="flex animate-fade animate-once animate-delay-[500ms]">
    <div class="flex-auto w-1 pt-3"></div>
    <div class="flex-row pt-5 justify-end">
      <button style="margin-left: 16px" @click="drawer = true" class="bg-white dark:bg-black bg-opacity-20 hover:bg-opacity-90 dark:bg-opacity-10">
        <svg xmlns="http://www.w3.org/2000/svg" width="1.5em" height="1.5em" viewBox="0 0 24 24">
          <path fill="currentColor" d="M4.5 17.27q-.213 0-.356-.145T4 16.768t.144-.356t.356-.143h15q.213 0 .356.144q.144.144.144.357t-.144.356t-.356.143zm0-4.77q-.213 0-.356-.144T4 11.999t.144-.356t.356-.143h15q.213 0 .356.144t.144.357t-.144.356t-.356.143zm0-4.77q-.213 0-.356-.143Q4 7.443 4 7.23t.144-.356t.356-.143h15q.213 0 .356.144T20 7.23t-.144.356t-.356.144z"></path>
        </svg>
      </button>
    </div>
  </div>

  <el-drawer
  v-model="drawer"
  class="drawer  lg:min-w-96 min-w-full"
  :direction="direction"
  size="50%"
>
  <!-- Drawer content goes here -->
    <div class="NavText ">
      <Titleblock />
      <Navigations />
      <UtilNav />
    </div>
    <div class="Footer">
      <Footer />
    </div>
</el-drawer>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import type { DrawerProps } from 'element-plus'
const drawer = ref(false)
const direction = ref<DrawerProps['direction']>('rtl')
</script>


<style>
.el-drawer  { background-color: #000000fc; color: #fa5ed3; }
.dark-mode .el-drawer { background-color: rgba(30, 30, 30, 0.98);  }

.fixed-drawer {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1000; /* Ensures it appears on top */
  background-color: rgba(255, 255, 255, 0.99);
}

.flex-row {
  display: flex;
  justify-content: flex-end;
}

.flex-row button {
  align-self: flex-start; /* Align the button at the top */
}

.mt-auto {
  margin-top: auto;
}

.NavText {
  margin-top: auto;
  font-family: "Space Mono", monospace;
}
</style>
