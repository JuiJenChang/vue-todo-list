<template>
  <div class="home">
    <h1>Home</h1>
    <router-link to="/todoList">Todo List Page</router-link>
  </div>
</template>

<script>
  export default {
    name: 'Home'
  }
</script>

<style lang="scss" scoped>
.home {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #bafaff;
}
h1 {
  font-size: 32px;
  font-weight: 600;
  margin-bottom: 20px;
  color: #388eff;
}
a {
  text-decoration: none;
  color: #ffa806;
}
</style>