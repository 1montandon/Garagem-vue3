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
          Os Melhores <span style="color: #ff3131; font-weight: 500">Carros</span> Roubados Perto de
          <span style="color: #ff3131; font-weight: 500">Você</span>
        </h1>
        <p>
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Sequi veritatis consectetur illo
          corporis libero commodi repellendus cum pariatur aliquid? A nam labore est explicabo nihil
          aliquid? Dicta itaque error at!Lorem ipsum dolor sit amet consectetur adipisicing elit.
          Sequi veritatis consectetur illo
        </p>
      </div>

      <div class="button">
        <a href="">Messi Viado </a>
      </div>
    </div>
    <div class="img">
      <img src="https://i.ibb.co/q0yfS61/FL2-G8-REW-WX.png" alt="FL2-G8-REW-WX" />
    </div>
  </div>
</template>
<style scoped>
.container {
  display: flex;
  padding: 0 7rem;
  margin-top: 5.5rem;
  justify-content: space-between;
}

.button {
  display: flex;
  background-color: #ff3131;
  height: 5em;
  width: 16em;
  border-radius: 10px;
  align-items: center;
  justify-content: center;

  & a {
    font-size: 1.7rem;
    font-weight: 400;
    color: white;
    text-decoration: none;
  }
}
.content {
  display: flex;
  width: 80%;

  flex-direction: column;
  gap: 1.3rem;
  & h1 {
    font-size: 7rem;
    font-weight: 500;
    line-height: 1em;
    width: 100%;
  }
  & p {
    font-size: 1.2rem;
    color: rgb(107, 107, 107);
    width: 80%;
  }
}
.text-content {
  display: flex;
  flex-direction: column;
  gap: 1em;
  width: 100%;
}
.img {
  width: 95%;
  margin-top: 4rem;
  & img {
    width: 100%;
  }
}

@media only screen and (max-width: 1240px) {
  .img {
    margin-top: 10rem;
    width: 70%;
  }
}
</style>
