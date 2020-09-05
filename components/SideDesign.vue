<template>
<div onselectstart="return false" class= "DesignTests">
      <div id="circle">
        <svg height="100" width="100">
     <circle cx="50" cy="50" r="40" stroke-width="4" ></circle>
    </svg>
    </div>
  <div class="Sideshades">

    <h1 class="ShaderProgrammierung">
    Shader-Programmeriung
    </h1>
</div>

    <div class= "Menusymbols">
      <div id="ersterbalken" class="balken"></div>
      <div id="zweiterbalken" class="balken"></div>
      <div id="dritterbalken" class="balken"></div>
    </div>

     <div onselectstart="return false" class= "Props">
      <div  onselectstart="return false" class="Propspoint">Point 1</div>
      <div  onselectstart="return false" class="Propspoint">Point 2</div>
      <div  onselectstart="return false" class="Propspoint">Point 3</div>
    </div>
  </div>



</template>

<script>
import anim from 'animejs/lib/anime.es.js';
export default
{
 
mounted(){
document.addEventListener('mousemove', e => {
  var  x = event.clientX;     // Get the horizontal coordinate
var  y = event.clientY;     // Get the vertical coordinate
anim({
  targets: '#circle',
  translateX: x,
   translateY: y,
});
});

}
}
</script>

<style scoped lang="scss">
#circle
{
  transform: rotate(45);
  width: 15vw;
  position: absolute;
  z-index: 1;
  mix-blend-mode:screen;
  fill: $myBlack;
}



.balken{
  width: auto;
  height: auto;
  margin: 1%;
  background-color: $myWhite;
  height: 40%;
}

#ersterbalken
{
  grid-row-start:2;
  grid-row-end:2;
  grid-column-start:2;
  grid-column-start:2;
}

#zweiterbalken
{
  width: 66%;
  grid-row-start:4;
  grid-row-end:4;
  grid-column-start:2;
  grid-column-start:2;
}
#dritterbalken
{
  grid-row-start:6;
  grid-row-end:6;
  grid-column-start:2;
  grid-column-start:2;
}

.Menusymbols{
    color: white;
  position: relative;
  display:grid;
  grid-template-columns:  2% 96% 2%;
  grid-template-rows: 0.5% 20% 12% 20% 12% 20% 0.5%; 
  justify-content: center;
  background-color:none;
  width: 60%;
  height: 50%;
  grid-row-start:2;
  grid-row-end:2;
  grid-column-start:1;
  grid-column-start:1;
  margin-left: 15%;
  font-size: 0.8em;
}

.Props
{
  color: $myYellow;
  grid-row-start:3;
  grid-row-end:3;
  grid-column-start:2;
  grid-column-start:2;
  font-family: proxima-nova, sans-serif;
  font-weight: lighter;
  font-style: normal;
  font-size: 1.7em;
  margin-top: 10%;
  cursor: pointer;
}
.Propspoint
{
  margin-top: 2%;
}
h1{
font-family: proxima-nova, sans-serif;
font-weight:normal;
font-style: normal;
  letter-spacing:5px;
  font-size:0.5em;
}
.ShaderProgrammierung {
  color:$myBlack;
  transform-origin:0% 110%;
  transform:rotate(90deg);
}
.Sideshades{
  grid-row-start:1;
  grid-row-end:6;
  grid-column-start:4;
  grid-column-end:5;
  background-color:$myWhite;

}

.DesignTests
{
width: 100vw;
height: 100vh;
font-size:5rem;
display: grid;
grid-template-columns:  10% 70% 15% 5%;
grid-template-rows: 2.5%  15%  70% 10% 2.5%; 
}
</style>
