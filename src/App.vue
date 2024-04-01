<template>
  <div>
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav>
    <router-view />
  </div>
</template>
<script>
export default {};
</script>
<style>
:root {
  --front-color: white;
  --gradient1-color: #8e2de2;
  --gradient2-color: #4a00e0;
}

html,
body {
  box-sizing: border-box;
  background: linear-gradient(
    to right,
    var(--gradient1-color),
    var(--gradient2-color)
  );
  color: var(--front-color);
  font-family: monospace;
  height: 100vh;
}

h1,
h2 {
  font-weight: normal;
  font-size: 4rem;
  margin-block: 0.5rem 3.5rem;
  text-align: center;
}
h2 {
  font-size: 3rem;
}

nav {
  background: black;
  padding: 2rem;
  text-align: center;
  width: 80vw;
  border-radius: 1rem;
  margin: 0 auto;
  font-size: 2rem;
}

nav a {
  font-size: 2rem;
  color: var(--front-color);
  text-decoration: none;
}

main {
  display: flex;
  justify-content: center;
  align-items: center;
}

.todo-article {
  width: 80vw;
  padding: 2rem;
  background: black;
  border-radius: 1rem;
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.2);
  margin-top: 2rem;
}
button {
  text-transform: uppercase;
  border: 5px solid var(--gradient2-color);
  border-radius: 0.5rem;
  background: black;
  color: var(--front-color);
  font-family: monospace;
  padding: 1rem;
  font-size: 2rem;
  cursor: pointer;
}
button:hover,
button:active,
button:focus,
select:hover,
select:active,
select:focus {
  border: 5px solid var(--gradient1-color);
}
</style>
