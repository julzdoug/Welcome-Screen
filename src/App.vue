<template>
  <header>
    <div>
<h1>Welcome to Opportunity</h1>
</div>
<div>
<span id="date">{{currentDate()}}</span>
</div>
</header>
<div class="row">
  <div class="menu">
    <dl>
       <dd>
        <div>
        <h3 class="Tims">ZeitPlan</h3>
        <h3 class="Task">Title</h3>
        <p class="Task">Task</p>
        </div>
      </dd>
      <dd>
        <h3 class="Tims">ZeitPlan</h3>
        <h3 class="Task">Title</h3>
        <p class="Task">Task</p>
      </dd>
      <dd>
        <h3 class="Tims">ZeitPlan</h3>
        <h3 class="Task">Title</h3>
        <p class="Task">Task</p>
      </dd>
    </dl>
      </div>
</div>
<footer class="footer">
  <img src="../src/assets/STZH_SEB_Logo.png" alt="">
  <img src="../src/assets/Opportunity.png" alt="">
  <img src="../src/assets/SAG_Logo_De.png" alt="">
</footer>
</template>

<script>
        export default {
  methods: {
    currentDate() {
      const current = new Date();
      const date = `${current.getDate()}.${current.getMonth()+1}.${current.getFullYear()}`;
      return date;
    }
  }
};
</script>

<style>
@import url('https://https://fonts.googleapis.com/css2?family=Inter:wght@500;900&display=swap%22');

*{
  font-family: "Inter",Arial, Helvetica, sans-serif;
}

h1 {
position: absolute;
top:0%;
max-width: 70%;
height:4%;
left: 5%;
font-style:normal;
font-weight: 900;
font-size: 400%;
line-height: 75px;
color: #323D4A;
}

body{
  background-color: #E8EFF4;
  max-height: 120rem;
  max-width: 63.75rem;
}

#date {
position:absolute;
width: 31%;
min-height: 4%;
left: 5%;
top: 5%;
margin-top: 5%;
font-style: normal;
font-weight: 500;
font-size: 400%;
line-height: 20%;
color: #9AA7B1;
}
.menu {
 margin-top: 20%;
}

.menu dd {
width: 90%;
height: 8.6%;
top: 20%;
left: 5%;
background-color: #0F05A0;
box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}

.Tims {
  color: #EB5E00;
  font-size: 200%;
  margin-left: 2%;
  text-justify: auto;
 
}
.Task {
  color: #FFBFAB;
  font-size: 200%;
  margin-left: 2%;
 
}
.footer {
  display: flex;
  justify-content: space-between;
  position: fixed;
  bottom:0;
  left:0;
  width:100%;
  padding:40px;
  background: #FFF;
}

.footer,img {
  height:50px;
}
</style>
