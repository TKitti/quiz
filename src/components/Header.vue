<template>
  <nav>
    <ul>
      <li class="right-border"><router-link to="/" exact>NEW QUIZ</router-link></li>
      <li class="left-border"><router-link to="/add" exact>QUESTIONS</router-link></li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: "app-header",
  data () {
    return {

    }
  }
}
</script>

<style scoped>
ul {
  list-style-type: none;
  text-align: center;
  margin: 0;
}
li {
  display: inline-block;
  background-color: #574f7d;
  border-radius: 10px;
  padding: 20px;
  border-bottom: 3px solid grey;
}
a {
  text-decoration: none;
  color: #dff0ea;
  font-weight: bold;
}
nav {
  width: 100%;
  margin: 0px auto;
}
.router-link-active {
  border-radius: 10px;
  padding: 20px;
  background: #95adbe;
  color: #fcf9ea;
}
.right-border {
  border-right: grey solid 3px;
  margin-right: 10px;
}
.left-border {
  border-left: grey solid 3px;
  margin-left: 10px;
}
</style>
