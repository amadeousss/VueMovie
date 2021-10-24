<template>
  <main>
    <router-view/>
  </main>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@100;300;400;700;900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@100;300;400;700;900&family=Roboto&display=swap');

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Lato', sans-serif;

  &::selection{
    background: transparentize(#42b883, 0.5);
  }
}

body{
  background-color: white;
}

a{
  text-decoration: none;
}

header{
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px 16px;
  box-shadow: 0px 0px 6px rgba(0,0,0,0.1);
}

h1{
  color: white;
  font-size: 28px;
  font-weight: 800;

  span{
    color: #42B883;
  }
}

</style>
