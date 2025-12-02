<script setup lang="ts">
import { ref, watch } from 'vue'

// Props
const props = defineProps<{
  open: boolean
}>()

// Emits
const emit = defineEmits<{
  'update:open': [value: boolean]
}>()

// Tab 配置
const tabs = [
  { id: 'settings', label: '设置', icon: 'i-heroicons-cog-6-tooth' },
  { id: 'help', label: '帮助', icon: 'i-heroicons-question-mark-circle' },
]

const activeTab = ref('settings')

// 关闭弹窗
function closeModal() {
  emit('update:open', false)
}

// 监听打开状态，重置 Tab
watch(
  () => props.open,
  (newVal) => {
    if (newVal) {
      activeTab.value = 'settings'
    }
  }
)
</script>

<template>
  <UModal :open="open" @update:open="emit('update:open', $event)">
    <template #content>
      <UCard class="w-full">
        <template #header>
          <div class="flex items-center justify-between">
            <h2 class="text-lg font-semibold text-gray-900 dark:text-white">全局设置</h2>
            <UButton icon="i-heroicons-x-mark" color="gray" variant="ghost" size="sm" @click="closeModal" />
          </div>
        </template>

        <!-- Tab 导航 -->
        <div class="mb-6 border-b border-gray-200 dark:border-gray-700">
          <div class="flex gap-1">
            <button
              v-for="tab in tabs"
              :key="tab.id"
              class="flex items-center gap-2 border-b-2 px-4 py-2.5 text-sm font-medium transition-colors"
              :class="[
                activeTab === tab.id
                  ? 'border-primary-500 text-primary-600 dark:text-primary-400'
                  : 'border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-300',
              ]"
              @click="activeTab = tab.id"
            >
              <UIcon :name="tab.icon" class="h-4 w-4" />
              <span>{{ tab.label }}</span>
            </button>
          </div>
        </div>

        <!-- Tab 内容 -->
        <div class="min-h-[300px]">
          <Transition name="fade" mode="out-in">
            <!-- 设置 Tab -->
            <div v-if="activeTab === 'settings'" key="settings" class="space-y-6">
              <!-- AI 服务配置 -->
              <div>
                <h3 class="mb-3 flex items-center gap-2 text-sm font-semibold text-gray-900 dark:text-white">
                  <UIcon name="i-heroicons-sparkles" class="h-4 w-4 text-violet-500" />
                  AI 服务配置
                </h3>
                <div class="rounded-lg border border-gray-200 bg-gray-50 p-4 dark:border-gray-700 dark:bg-gray-800/50">
                  <p class="text-sm text-gray-600 dark:text-gray-400">
                    AI 配置已移至 AI 对话探索页面，请在该页面右上角点击「配置」按钮进行设置。
                  </p>
                  <div class="mt-3 flex items-center gap-2 text-xs text-gray-400">
                    <UIcon name="i-heroicons-arrow-right" class="h-3 w-3" />
                    <span>群聊分析 → AI → 对话式探索 → 配置</span>
                  </div>
                </div>
              </div>

              <!-- 外观设置 -->
              <div>
                <h3 class="mb-3 flex items-center gap-2 text-sm font-semibold text-gray-900 dark:text-white">
                  <UIcon name="i-heroicons-paint-brush" class="h-4 w-4 text-blue-500" />
                  外观设置
                </h3>
                <div class="rounded-lg border border-gray-200 bg-gray-50 p-4 dark:border-gray-700 dark:bg-gray-800/50">
                  <div class="flex items-center justify-between">
                    <div>
                      <p class="text-sm font-medium text-gray-900 dark:text-white">深色模式</p>
                      <p class="text-xs text-gray-500 dark:text-gray-400">跟随系统自动切换</p>
                    </div>
                    <UBadge color="gray" variant="soft">自动</UBadge>
                  </div>
                </div>
              </div>

              <!-- 数据设置 -->
              <div>
                <h3 class="mb-3 flex items-center gap-2 text-sm font-semibold text-gray-900 dark:text-white">
                  <UIcon name="i-heroicons-circle-stack" class="h-4 w-4 text-green-500" />
                  数据设置
                </h3>
                <div class="rounded-lg border border-gray-200 bg-gray-50 p-4 dark:border-gray-700 dark:bg-gray-800/50">
                  <p class="text-sm text-gray-500 dark:text-gray-400">更多数据相关设置即将推出...</p>
                </div>
              </div>
            </div>

            <!-- 帮助 Tab -->
            <div v-else-if="activeTab === 'help'" key="help" class="space-y-6">
              <!-- 关于 -->
              <div>
                <h3 class="mb-3 flex items-center gap-2 text-sm font-semibold text-gray-900 dark:text-white">
                  <UIcon name="i-heroicons-information-circle" class="h-4 w-4 text-blue-500" />
                  关于 ChatLab
                </h3>
                <div class="rounded-lg border border-gray-200 bg-gray-50 p-4 dark:border-gray-700 dark:bg-gray-800/50">
                  <div class="flex items-center gap-3">
                    <div
                      class="flex h-12 w-12 items-center justify-center rounded-xl bg-gradient-to-br from-violet-500 to-purple-600"
                    >
                      <UIcon name="i-heroicons-chat-bubble-left-right" class="h-6 w-6 text-white" />
                    </div>
                    <div>
                      <p class="text-sm font-semibold text-gray-900 dark:text-white">ChatLab</p>
                      <p class="text-xs text-gray-500 dark:text-gray-400">聊天记录分析工具</p>
                      <p class="mt-1 text-xs text-gray-400">版本 0.1.0</p>
                    </div>
                  </div>
                </div>
              </div>

              <!-- 快捷键 -->
              <div>
                <h3 class="mb-3 flex items-center gap-2 text-sm font-semibold text-gray-900 dark:text-white">
                  <UIcon name="i-heroicons-command-line" class="h-4 w-4 text-green-500" />
                  快捷键
                </h3>
                <div class="rounded-lg border border-gray-200 bg-gray-50 p-4 dark:border-gray-700 dark:bg-gray-800/50">
                  <div class="space-y-2 text-sm">
                    <div class="flex items-center justify-between">
                      <span class="text-gray-600 dark:text-gray-400">发送消息</span>
                      <div class="flex gap-1">
                        <kbd class="rounded bg-gray-200 px-2 py-0.5 text-xs dark:bg-gray-700">Enter</kbd>
                      </div>
                    </div>
                    <div class="flex items-center justify-between">
                      <span class="text-gray-600 dark:text-gray-400">换行</span>
                      <div class="flex gap-1">
                        <kbd class="rounded bg-gray-200 px-2 py-0.5 text-xs dark:bg-gray-700">Shift</kbd>
                        <span class="text-gray-400">+</span>
                        <kbd class="rounded bg-gray-200 px-2 py-0.5 text-xs dark:bg-gray-700">Enter</kbd>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- 常见问题 -->
              <div>
                <h3 class="mb-3 flex items-center gap-2 text-sm font-semibold text-gray-900 dark:text-white">
                  <UIcon name="i-heroicons-chat-bubble-left-ellipsis" class="h-4 w-4 text-amber-500" />
                  常见问题
                </h3>
                <div class="space-y-2">
                  <details
                    class="group rounded-lg border border-gray-200 bg-gray-50 dark:border-gray-700 dark:bg-gray-800/50"
                  >
                    <summary
                      class="flex cursor-pointer items-center justify-between px-4 py-3 text-sm font-medium text-gray-900 dark:text-white"
                    >
                      如何导入聊天记录？
                      <UIcon
                        name="i-heroicons-chevron-down"
                        class="h-4 w-4 text-gray-400 transition-transform group-open:rotate-180"
                      />
                    </summary>
                    <div
                      class="border-t border-gray-200 px-4 py-3 text-sm text-gray-600 dark:border-gray-700 dark:text-gray-400"
                    >
                      在「实用工具」页面选择「导入聊天记录」，支持 QQ 导出的 txt 格式文件。
                    </div>
                  </details>

                  <details
                    class="group rounded-lg border border-gray-200 bg-gray-50 dark:border-gray-700 dark:bg-gray-800/50"
                  >
                    <summary
                      class="flex cursor-pointer items-center justify-between px-4 py-3 text-sm font-medium text-gray-900 dark:text-white"
                    >
                      如何使用 AI 功能？
                      <UIcon
                        name="i-heroicons-chevron-down"
                        class="h-4 w-4 text-gray-400 transition-transform group-open:rotate-180"
                      />
                    </summary>
                    <div
                      class="border-t border-gray-200 px-4 py-3 text-sm text-gray-600 dark:border-gray-700 dark:text-gray-400"
                    >
                      首先在 AI 对话探索页面配置 API Key（支持 DeepSeek、Qwen 等），然后即可开始提问。
                    </div>
                  </details>

                  <details
                    class="group rounded-lg border border-gray-200 bg-gray-50 dark:border-gray-700 dark:bg-gray-800/50"
                  >
                    <summary
                      class="flex cursor-pointer items-center justify-between px-4 py-3 text-sm font-medium text-gray-900 dark:text-white"
                    >
                      数据保存在哪里？
                      <UIcon
                        name="i-heroicons-chevron-down"
                        class="h-4 w-4 text-gray-400 transition-transform group-open:rotate-180"
                      />
                    </summary>
                    <div
                      class="border-t border-gray-200 px-4 py-3 text-sm text-gray-600 dark:border-gray-700 dark:text-gray-400"
                    >
                      所有数据保存在本地「文档/ChatLab」目录下，包括聊天记录数据库和 AI 配置。
                    </div>
                  </details>
                </div>
              </div>
            </div>
          </Transition>
        </div>
      </UCard>
    </template>
  </UModal>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.15s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
