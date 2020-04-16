<template>
<div id="app">
  <div id="page">
    <div id="menu">
      <div id="none">
      </div>
      <div id="title">
        <router-link to="/">
          <div class="logo">
            <img src="/travel.svg">
          </div>
        </router-link>
        <div class="title">
          <h1>Travel Journal</h1>
        </div>
      </div>
      <div id="side">
        <router-link to="/browse">
          <div class="menu-item">
            <router-link to="/">
              <p>Home</p>
            </router-link>

          </div>
        </router-link>
        <router-link to="/admin">
          <div class="menu-item">
            <p>Admin</p>
          </div>
        </router-link>
      </div>
    </div>
  </div>




  <div class="content">
    <router-view />
  </div>


  <div class="footer">
    <a href="https://github.com/BYU-CS-260-Winter-2020/lab-4-museum-of-ordinary-objects-Shawn45.git">Github Repository</a>
  </div>
</div>
</template>



<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}
#page {
    min-height: 100%;
    margin-left: 100px;
    margin-right: 100px;
}

#menu {

  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-column-gap: 5px;
  grid-template-areas: "none title side";
  margin-bottom: 50px;
  padding-top: 50px;
}
.menu-item {
  padding: 10px;
}


.logo img {
  height: 90px;
  margin-right: 40px;

}

#title {
  margin-top: 5px;
  display:flex;
  justify-content: center;
}
#side {
  display:flex;
  justify-content:flex-end;
}

.title h1 {
  font-size: 30px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  display: flex;
  justify-content: center;
  height: 50px;
  padding: 20px 100px 0px 100px;
  background: #e3e3e3;
  font-size: 12px;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>
