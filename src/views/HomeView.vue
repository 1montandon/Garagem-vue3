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
          aliquid? Dicta itaque error at!Lorem ipsum dolor sit amet consectetur adipisicing elit. 
          Sequi veritatis consectetur illo
         
        </p>
      </div>

      <div class="button">
        <a href="">Messi Viado </a>
      </div>
    </div>
    <div class="img"><img src="https://i.ibb.co/q0yfS61/FL2-G8-REW-WX.png" alt="FL2-G8-REW-WX" /> </div>
    
  </div>
</template>
<style scoped>
.container {
  display: flex;
  width: 100%;
  padding: 0 7rem;
  /*height: 23rem;
  /*height: 370px;*/
  margin-top: 5.5rem;
  justify-content: space-between;
}

.button {
  display: flex;
  background-color: #ff3131;
  height: 3.5em;
  width: 10em;
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
  /*width: 38rem;*/
  width: 60%;

  flex-direction: column;
  gap: 2.2em;
  & h1 {
    /* font-size: 4.6em; */
    font-size: 4.6em;
    font-weight: 500;
    line-height: 1em;
    width: 100%;

  }
  & p {
    font-size: 0.8em;
    color: rgb(107, 107, 107);
    width: 80%;
  }
}
.text-content{
  display: flex;
  flex-direction: column;
  gap: 1em;
  width: 120%;
}
.img{
  width: 95%;
  margin-top: 4em;
  & img{
    width: 100%;
  }
}

</style>
