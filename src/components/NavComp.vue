<template>
  <div class="app-nav flex-col">
    <el-menu
      class="nav-wrapper flex-1"
      :default-active="activeNav"
      :default-openeds="['3', '4']"
    >
      <div class="avatar"></div>
      <template v-for="(item, row) in navConfig" :key="row">
        <el-sub-menu v-if="item.children" :index="`${row + 1}`">
          <template #title>
            <el-icon>
              <component :is="item.icon"></component>
            </el-icon>
            <span>{{ item.title }}</span>
          </template>
          <el-menu-item-group>
            <el-menu-item
              v-for="(child, col) in item.children"
              :key="`${row + 1}-${col + 1}`"
              :index="`${row + 1}-${col + 1}`"
              @click="handleClick(child, `${row + 1}-${col + 1}`)"
            >{{ child.title }}</el-menu-item>
          </el-menu-item-group>
        </el-sub-menu>
        <el-menu-item v-else :index="`${row + 1}`" @click="handleClick(item, `${row + 1}`)">
          <el-icon>
            <component :is="item.icon"></component>
          </el-icon>
          <span>{{ item.title }}</span>
        </el-menu-item>
      </template>
    </el-menu>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import { useRouter, useRoute } from 'vue-router'

const navConfig = [
  {
    title: '网站导航',
    icon: 'Link',
    path: '/'
  },
  {
    title: '插件导航',
    icon: 'Key',
    path: '/plugins'
  },
  {
    title: '前端工具',
    icon: 'Document',
    children: [
      {
        title: '代码格式化',
        path: '/beautify'
      },
      {
        title: '二维码生成',
        path: '/qrcode'
      },
      {
        title: '二维码解析',
        path: '/parse-qrcode'
      },
      {
        title: '正则表达式',
        path: '/regexp'
      },
      {
        title: '时间、颜色转换',
        path: '/transfer'
      },
      {
        title: 'PS投影转CSS3',
        path: '/shadow'
      },
      {
        title: 'CSS三角形生成器',
        path: '/triangle'
      },
    ]
  },
  {
    title: '其他工具',
    icon: 'FolderOpened',
    children: [
      {
        title: 'm3u8视频下载',
        path: '/m3u8'
      },
      
      // {
      //   title: '图片拼接',
      //   path: '/stitch'
      // },
    ]
  },
  // {
  //   title: '影视资源',
  //   icon: 'Film',
  //   path: '/movie'
  // },
]

const activeNav = ref('1')
const route = useRoute()
const queryActive = computed(() => (route.query.a || '1'))
watch(queryActive, newVal => activeNav.value = newVal)

const router = useRouter()
const handleClick = (opts, index) => {
  const { url, path } = opts
  if (url) {
    window.open(url)
  } else {
    router.push({ path, query: { a: index } })
    activeNav.value = index
  }
}
</script>

<style lang="less" scoped>
.app-nav {
  width: 200px;
  height: 100%;
  margin-right: 20px;
  overflow: hidden;
  user-select: none;
  .avatar {
    width: 120px;
    height: 120px;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    background-image: url(../assets/avatar.png);
    background-color: transparent;
    margin: 20px auto;
  }
  .nav-wrapper {
    border-radius: 14px;
    box-shadow: rgba(0, 0, 0, 0.04) 0px 12px 32px 4px;
    border-right: none !important;
    // margin-top: 20px;
    :deep(.el-menu-item) {
      font-size: 15px;
    }
    :deep(.el-sub-menu__title) {
      font-size: 15px;
    }
  }
}
</style>
