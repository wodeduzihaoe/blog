<template>
  <div class="home-container">
    <el-container>
      <el-header>
        <div class="header-content">
          <h1>我的博客</h1>
          <div class="user-info">
            <span>欢迎，{{ userInfo.username }}</span>
            <el-button type="danger" size="small" @click="handleLogout">退出登录</el-button>
          </div>
        </div>
      </el-header>
      <el-main>
        <el-card>
          <template #header>
            <div class="card-header">
              <span>欢迎使用博客系统</span>
            </div>
          </template>
          <div class="welcome-content">
            <p>您已成功登录！</p>
            <p>用户ID: {{ userInfo.id }}</p>
            <p>用户名: {{ userInfo.username }}</p>
            <p>邮箱: {{ userInfo.email }}</p>
          </div>
        </el-card>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { ElMessage } from 'element-plus'
import { authAPI } from '../api/auth'

export default {
  name: 'Home',
  setup() {
    const router = useRouter()
    const userInfo = ref({
      id: '',
      username: '',
      email: ''
    })

    const loadUserInfo = async () => {
      try {
        const userStr = localStorage.getItem('user')
        if (userStr) {
          userInfo.value = JSON.parse(userStr)
        } else {
          const response = await authAPI.getUser()
          if (response.success) {
            userInfo.value = response.user
            localStorage.setItem('user', JSON.stringify(response.user))
          }
        }
      } catch (error) {
        ElMessage.error('获取用户信息失败')
        router.push('/login')
      }
    }

    const handleLogout = () => {
      localStorage.removeItem('token')
      localStorage.removeItem('user')
      ElMessage.success('已退出登录')
      router.push('/login')
    }

    onMounted(() => {
      loadUserInfo()
    })

    return {
      userInfo,
      handleLogout
    }
  }
}
</script>

<style scoped>
.home-container {
  min-height: 100vh;
}

.el-header {
  background-color: #409eff;
  color: white;
  line-height: 60px;
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 100%;
}

.header-content h1 {
  margin: 0;
  font-size: 24px;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 15px;
}

.el-main {
  padding: 20px;
  background-color: #f5f5f5;
}

.welcome-content {
  padding: 20px;
}

.welcome-content p {
  margin: 10px 0;
  font-size: 16px;
  color: #333;
}
</style>

