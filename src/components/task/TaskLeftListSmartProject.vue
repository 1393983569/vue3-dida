<script setup lang="ts">
import { Icon } from '@iconify/vue'
import { reactive, ref } from 'vue'
import { NInput, NModal, NPopover } from 'naive-ui'
import type { Ref } from 'vue'

import {
  SmartProjectNames,
  editHideSmartProject,
  useProjectSelectedStatusStore,
  useTaskStore,
} from '@/store'

interface TaskListType {
  key: number
  icon: string
  title: `${SmartProjectNames}`
}

const taskList = reactive<TaskListType[]>([
  {
    key: 1,
    icon: 'material-symbols:check-box',
    title: SmartProjectNames.Complete,
  },
  {
    key: 2,
    icon: 'mdi:close-box',
    title: SmartProjectNames.Failed,
  },
  {
    key: 3,
    icon: 'material-symbols:delete',
    title: SmartProjectNames.Trash,
  },
  {
    key: 4,
    icon: 'material-symbols:text-snippet-rounded',
    title: SmartProjectNames.Abstract,
  },
])

const showEditBox: Ref<any> = ref(false)

const editTarget: Ref<any> = ref(null)

const smartProjectName: Ref<any> = ref(null)

const selected = 'bg-[#E7F5EE] dark:bg-[#233633]'

const taskStore = useTaskStore()
const projectSelectedStatusStore = useProjectSelectedStatusStore()

const handleTaskItemClick = (projectName: string, key: number) => {
  taskStore.changeCurrentActiveProject(projectName)
  projectSelectedStatusStore.changeSelectedKey([key])
}

function handleTaskItemSettings(name: string) {
  editHideSmartProject(name, true)
  // console.log()
}
</script>

<template>
  <ul>
    <li
      v-for="item in taskList"
      :key="item.key"
      li_common
      pl-4
      pr-2
      hover="bg-[#F3F3F5] dark:bg-[#2D2D30]"
      :class="
        projectSelectedStatusStore.selectedKey[0] === item.key ? selected : ''
      "
      @click="handleTaskItemClick(item.title, item.key)"
    >
      <div flex>
        <Icon
          :icon="item.icon"
          width="20"
          class="color-[#9D9FA3]"
          dark="color-white-b"
        />
        <span class="ml-2">{{ item.title }}</span>
      </div>
      <NPopover
        placement="right-start"
        :show-arrow="false"
        trigger="hover"
        style="border: 1px solid rgb(255 255 255 / 10%);width: 150px"
      >
        <template #trigger>
          <Icon
            v-show="projectSelectedStatusStore.selectedKey[0] === item.key"
            icon="material-symbols:more-horiz"
            width="20"
            class="color-[#9D9FA3]"
            dark="color-white"
          />
        </template>
        <div class="cursor-pointer" @click="handleTaskItemSettings(item.title)">
          隐藏
        </div>
      </NPopover>
    </li>
  </ul>
</template>

<style scoped></style>
