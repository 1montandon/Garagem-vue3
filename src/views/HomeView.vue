<script setup>
import { onMounted } from 'vue'
import { PassageUser } from '@passageidentity/passage-elements/passage-user'
import { useAuthStore } from '@/stores/auth'

const authStore = useAuthStore()

const getUserInfo = async () => {
  try {
    const authToken = localStorage.getItem('psg_auth_token')
    const passageUser = new PassageUser(authToken)
    const user = await passageUser.userInfo(authToken)
    if (user) {
      await authStore.setToken(authToken)
    } else {
      authStore.unsetToken()
    }
  } catch (error) {
    authStore.unsetToken()
  }
}

onMounted(() => {
  getUserInfo()
})
</script>

<template>
  <div class="container">
    <div class="content">
      <div class="text-content">
        <h1>
          Os Melhores <span style="color: #ff3131; font-weight: 500">Carros</span> Roubados da
          Região
        </h1>
        <p>
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Sequi veritatis consectetur illo
          corporis libero commodi repellendus cum pariatur aliquid? A nam labore est explicabo nihil
          aliquid? Dicta itaque error at!
        </p>
      </div>

      <div class="button">
        <a href="">Messi Viado </a>
      </div>
    </div>

    <img src="https://i.ibb.co/q0yfS61/FL2-G8-REW-WX.png" alt="FL2-G8-REW-WX" />
  </div>
</template>
<style scoped>
.container {
  display: flex;
  width: 1300px;
  height: 370px;
  margin-top: 100px;
  justify-content: space-between;
}

.button {
  display: flex;
  background-color: #ff3131;
  height: 50px;
  width: 160px;
  border-radius: 10px;
  align-items: center;
    justify-content: center;

  & a {
    font-size: 1em;
    font-weight: 500;
    color: white;
    text-decoration: none;
  }
}
.content {
  display: flex;
  width: 700px;
  flex-direction: column;
  gap: 35px;
  & h1 {
    font-size: 4.8em;
    font-weight: 500;
    line-height: 70px;
  }
  & p {
    font-size: 0.8em;
    color: rgb(107, 107, 107);
    width: 600px;
  }
}
.text-content{
  display: flex;
  flex-direction: column;
  gap: 15px;
}
img{
  height: 350px;
  margin: 50px;
}
</style>
