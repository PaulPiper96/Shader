<template>
  <div class="container">
<div class="grid">
<SideDesign></SideDesign>
<div onselectstart="return false" class="Eventim">
  <h1 class="clicknext">Next</h1>
<div class="rect"></div>
 </div>
</div>
  </div>

</template>

<script>
import * as THREE from 'three'
import {OrbitControls} from "three/examples/jsm/controls/OrbitControls";
import anim from 'animejs/lib/anime.es.js';
import SideDesign from '~/components/SideDesign.vue'

import MyImage from '~/assets/ole dicke cam2.jpg'

export default {
 head: {
    link: [
      {
        rel: 'Fonts',
        href: 'https://fonts.googleapis.com/css?family=Roboto&display=swap'
      }
    ]
  },

mounted(){
var x;
var y;


  var scene = new THREE.Scene();
			var camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
			var renderer = new THREE.WebGLRenderer();
			renderer.setSize(  window.innerWidth, window.innerHeight, 16, 16);
      document.body.appendChild( renderer.domElement );
scene.background = new THREE.Color( 0xffffff );
 var geometry2 = new  THREE.PlaneBufferGeometry( window.innerWidth,window.innerWidth);
function vertexShader() {
  return `
  varying vec2 vMouse;
varying vec2 vUv;
uniform float u_X;
uniform float u_Y;
    void main() {
        vUv = uv;
vec3 pos = vec3(position);
pos.x= pos.x+u_X*100.;
pos.y= pos.y+u_Y*-50.;
 vMouse= vec2(u_X,(-u_Y+1.0));
  vec2 u_resolution =vec2 (1920,1080);
      gl_Position =   projectionMatrix * 
                        modelViewMatrix * 
                        vec4(pos,1.0);
    }

  `
}


/*-------------------------------------------------------------------------------------------------
---------------------------------------Fragment Shader of the Cube--------------------------------
--------------------------------------------------------------------------------------------------*/

function cubefragment(){



  return `
  uniform sampler2D texture1;
  uniform sampler2D texture2;
  uniform float u_Time;
   uniform bool trigger;
  varying vec2 vUv;

float random (in vec2 st) {
    return fract(sin(dot(st.xy,
                         vec2(12.9898,78.233)))
                 * 43758.5453123);
}

float noise (in vec2 st) {
    vec2 i = floor(st);
    vec2 f = fract(st);

    // Four corners in 2D of a tile
    float a = random(i);
    float b = random(i + vec2(1.0, 0.0));
    float c = random(i + vec2(0.0, 1.0));
    float d = random(i + vec2(1.0, 1.0));

    // Smooth Interpolation

    // Cubic Hermine Curve.  Same as SmoothStep()
    vec2 u = f*f*(3.0-2.0*f);
    // u = smoothstep(0.,1.,f);

    // Mix 4 coorners percentages
    return mix(a, b, u.x) +
            (c - a)* u.y * (1.0 - u.x) +
            (d - b) * u.x * u.y;
}
      void main() {


        vec4 firstimage= texture2D(texture1,vUv);
        vec4 secondimage= texture2D(texture2,vUv);
    vec2 meshdimension = vec2(vUv);
    if(trigger==true){
      float Zeitsmooth= u_Time*0.05;
      meshdimension.x=noise(meshdimension)/Zeitsmooth;
       meshdimension.y=noise(meshdimension)/Zeitsmooth;
       vec4 miximage= texture2D(texture2,vUv);

       meshdimension.y= mix(meshdimension.y,miximage.r/Zeitsmooth, 0.7);
       meshdimension.x= mix(meshdimension.x,miximage.r/Zeitsmooth, 0.7);

       secondimage=  vec4(mix(texture2D(texture2, meshdimension),miximage,Zeitsmooth));
      }

    gl_FragColor =secondimage;
      }
  `
}


/*-------------------------------------------------------------------------------------------------
---------------------------------------Fragment Shader of the Circles--------------------------------
--------------------------------------------------------------------------------------------------*/
function fragmentShader(){



  return `


      // Author @patriciogv - 2015
// http://patriciogonzalezvivo.com

#ifdef GL_ES
precision mediump float;
#endif
  uniform vec2 u_resolution;
 varying vec2 vMouse;
 varying vec2 vUv;
uniform float u_Time;
uniform float u_smoothedtome;
/*_____________________________________________________CREATE A CIRCLE SHAPE TO GIVEN PARAMETERS_________________________________________________*/
float circleShape(vec2 position, float radius, vec2 Mouse)
{
  return step(radius, length(position-Mouse));
}
/*_____________________________________________________CREATE A CIRCLE SHAPE TO GIVEN PARAMETERS_________________________________________________*/





/*MAIN METHOD THAT RETURNS THE FRAGMENT SHADER*/

void main(){
// -----------------------------------------------------------------------------------------------------------------
//smoothtime and randomize it
float time=u_Time*1.2;
float amplitude = 1.5;
float frequency = 0.15;
float x= time;
float y = sin(x * frequency);
float t = 0.01*(time*-0.07);
y += sin(x*frequency*2.1 + t)*4.5;
y += sin(x*frequency*1.72 + t*1.121)*4.0;
y = sin(x*frequency*2.221 + t*0.437)*5.0;
y += sin(x*frequency*3.1122+ t*4.269)*2.5;
y *= amplitude*0.2;
// -----------------------------------------------------------------------------------------------------------------







//General 
// -----------------------------------------------------------------------------------------------------------------
vec2 st = gl_FragCoord.xy/u_resolution;
float pct = 0.0;
// a. The DISTANCE from the pixel to the center
pct = distance(st,vec2(vMouse));
// -----------------------------------------------------------------------------------------------------------------
//General 



//Middleshape
// -----------------------------------------------------------------------------------------------------------------
vec2 kurzwahl=st;
float bewegungsfaktor=y;
bewegungsfaktor= bewegungsfaktor/100.;
float  pattern=step(sin(st.y+bewegungsfaktor*0.005)+tan(st.x/bewegungsfaktor)+cos(st.x/bewegungsfaktor),0.2);
float letssee = circleShape(vec2(st.x,st.y),0.2,vec2 (0.5, 0.3+bewegungsfaktor));
vec3 combined= vec3(letssee+ pattern);
//Middleshape
// -----------------------------------------------------------------------------------------------------------------


//Make more Circles
// -----------------------------------------------------------------------------------------------------------------
pattern=step(sin(st.x+bewegungsfaktor*0.005)+tan(st.y/bewegungsfaktor)+cos(st.y/bewegungsfaktor),0.2);
vec2 makemousealittlesmoother= vMouse*0.5;
float secondcircle = circleShape(vec2(st.x,st.y),0.05,vec2 (0.4+makemousealittlesmoother.x, 0.07+makemousealittlesmoother.y));
vec3 secondcircleshape= vec3( secondcircle+ pattern);

combined=secondcircleshape*combined;




//Make more Circles
// -----------------------------------------------------------------------------------------------------------------
makemousealittlesmoother= vMouse*0.65;
secondcircle = circleShape(vec2(st.x,st.y),0.075,vec2 (0.5+makemousealittlesmoother.x, 0.05+makemousealittlesmoother.y));
secondcircleshape= vec3( secondcircle+ pattern);
combined=secondcircleshape*combined;
//Make more Circles
// -----------------------------------------------------------------------------------------------------------------

//Make more Circles
// -----------------------------------------------------------------------------------------------------------------
makemousealittlesmoother= vMouse*0.85;
secondcircle = circleShape(vec2(st.x,st.y),0.075,vec2 (0.85-makemousealittlesmoother.x, 0.06+makemousealittlesmoother.y));
secondcircleshape= vec3( secondcircle+ pattern);
combined=secondcircleshape*combined;
//Make more Circles
// -----------------------------------------------------------------------------------------------------------------







//Putting it all together
// -----------------------------------------------------------------------------------------------------------------
combined= vec3 (1.-combined.r,1.- combined.g, 1.-combined.b);
  gl_FragColor = vec4(combined, 1.0 );  
//Putting it all together
// -----------------------------------------------------------------------------------------------------------------


}
  `
}
var imagecounter=0;
var images= ["Zeichenfläche 1.2.jpg", "Zeichenfläche 1.1.jpg", "Zeichenfläche 1.3.jpg"];






var currentdate = new Date();
var imgtex = new THREE.TextureLoader();

 let uniforms = {
   u_X:{type:'float', value: 1.0},
    u_Y:{type:'float', value: 1.0},
    u_resolution: { type: "v2", value: new THREE.Vector2()},
       u_Time:{type:'float', value: 0.2},
    u_smoothedtome: {type:'float', value:0.4},
    }
 let custommaterial =  new THREE.ShaderMaterial({
    uniforms: uniforms,
    fragmentShader: fragmentShader(),
    vertexShader: vertexShader(),
    wireframe:false,
  })
  var realplane = new THREE.Mesh( geometry2, custommaterial);
      scene.add(realplane );    
       var object = scene.children[ 0 ];

     object.material.uniforms.u_resolution.value.x = window.innerWidth;
      object.material.uniforms.u_resolution.value.y = window.innerHeight*2;

/*---------------------------------------------------------------------------------
---------------------Create My Second Object--------------------------------------------------
------------------------------------------------------------------------------------------*/
var cube = new  THREE.BoxBufferGeometry( window.innerWidth/6,window.innerWidth/6,window.innerWidth/6);

let seconduniforms = {
   texture1: {value:imgtex.load(images[1] )},
   texture2: {value:imgtex.load(images[0] )},
     u_Time:{type:'float', value: 0.2},
     trigger:{type:'bool', value:false},
    }

 let cubematerialaterial =  new THREE.ShaderMaterial({
    uniforms: seconduniforms,
    fragmentShader: cubefragment(),
    vertexShader: vertexShader(),
    wireframe:false,
  });
var cubemesh = new THREE.Mesh(cube,cubematerialaterial);
scene.add(cubemesh);
 var cube_object = scene.children[1];
  cube_object.translateZ( 63);
  object.translateZ( -53);
/*---------------------------------------------------------------------------------
---------------------Create My Second Object--------------------------------------------------
------------------------------------------------------------------------------------------*/ 
var light = new THREE.AmbientLight( 0xf0f0f0 ); // soft white light
scene.add( light );



document.addEventListener('mousemove', e => {
  if(x!=event.clientX){
    x = event.clientX;     // Get the horizontal coordinate
 y = event.clientY;     // Get the vertical coordinate
   object.material.uniforms.u_Y.value= y / window.innerHeight;
  object.material.uniforms.u_X.value=e.pageX/window.innerWidth;
 }
});



/*-------------------------Glitch the Image--------------------------------- */
function loadnewImage(counter)
{
  cube_object.material.uniforms. trigger.value=true;
   cube_object.material.needsUpdate = true;
  cube_object.material.uniforms.u_Time.value+=10.;
   cube_object.material.uniforms.texture2.value=new THREE.TextureLoader().load(images[counter]);
}
 /*-------------------------Glitch the Image--------------------------------- */

    camera.position.z =window.innerHeight/2;
 var  secs=0;
var tl = anim.timeline({
  duration: 750,
  secs: secs++,
  easing: 'linear'
});


document.getElementsByClassName("Eventim")[0].addEventListener("click",function nextImage(e){
setTimeout(function(){cube_object.material.uniforms. trigger.value=false}, 1000);
if(imagecounter==images.length-1)
{
  imagecounter=0;
}else if(imagecounter<=images.length-1)
{
  imagecounter++;
}
  console.log("an der Stelle" ,imagecounter);
loadnewImage(imagecounter);
});



      function render() {
     cube_object.rotation.x += 0.001;
		cube_object.rotation.y += 0.001;
      renderer.render( scene, camera)
      }

      var objecttime=  object.material.uniforms.u_Time.value;
      var cubetime= cube_object.material.uniforms.u_Time.value;
			var animate = function (time) {


      /*--------------------Animation Methd------ */  
        requestAnimationFrame( animate );
               time= Math.floor(time);
       // if(time%3===0){
         /*-------------Update the timeline-------------------- */
       var tl = anim.timeline({
  duration:3000,
  secs: secs= secs+0.2,
  objecttime: object.material.uniforms.u_Time.value= secs * 0.13,
  cubetime:cube_object.material.uniforms.u_Time.value=secs%10,
  easing: 'linear'
});

         var wirreszeug=secs;
	    	render();
      };

      animate();
}}
</script>

<style lang="scss">
.container {
  position: absolute;
  z-index: 0;
  margin: 0 auto;
  min-height: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  overflow: hidden;
}
.Pfeil
{
  position: absolute;
  color: $myWhite;
  font-size:2rem;
  z-index: 2;
  margin-top: 90vh;
   margin-left: 70vw;
  cursor: pointer;
}
.grid
{
  background-color: none;
  z-index: 2;
  position: relative;
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-wrap:wrap;
}
.Eventim
{
  position: absolute;
  color: $myWhite;
  font-size:2rem;
  z-index: 4;
  margin-top: 90vh;
   margin-left:5%;
  cursor: pointer;
  font-family: proxima-nova, sans-serif;
}
.rect
{
  position: relative;
  z-index: -2;
  margin-top:-1.5rem;
  width: 5rem;
  height: 5rem;
  background-color: $myBlack;
  margin-left: 15%;
}
.clicknext{
  position: absolute;
  z-index: 4;
}

</style>
