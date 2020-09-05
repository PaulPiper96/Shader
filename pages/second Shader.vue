<template>
  <div class="container">
    <div>
<div class= "3jsContainer"></div>

   <script > </script>

      </div>
    </div>
  

</template>

<script>
import * as THREE from 'three'
import {OrbitControls} from "three/examples/jsm/controls/OrbitControls";
import anim from 'animejs/lib/anime.es.js';


import MyImage from '~/assets/ole dicke cam2.jpg'

export default {
mounted(){
var x;
var y;


  var scene = new THREE.Scene();
			var camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
			var renderer = new THREE.WebGLRenderer();
			renderer.setSize(  window.innerWidth, window.innerHeight, 16, 16);
      document.body.appendChild( renderer.domElement );
   var controls = new OrbitControls( camera, renderer.domElement );
scene.background = new THREE.Color( 0xffffff );
  var geometry = new  THREE.BoxBufferGeometry( window.innerWidth/5,window.innerWidth/5 ,window.innerWidth/5);
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
	vec2 st = vUv/u_resolution;
  float pct = 0.0;

    // a. The DISTANCE from the pixel to the center
    pct = distance(st,vec2(vMouse));

    vec3 color = vec3(pct);
    pos= pos+color;




        gl_Position =   projectionMatrix * 
                        modelViewMatrix * 
                        vec4(pos,1.0);
    }

  `
}

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
uniform sampler2D texture1;
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
float time=u_Time*0.2;
float amplitude = 2.;
float frequency = 1.;
float x= time;
float y = sin(x * frequency);
float t = 0.01*(-time*-0.07);
y += sin(x*frequency*2.1 + t)*4.5;
y += sin(x*frequency*1.72 + t*1.121)*4.0;
y += sin(x*frequency*2.221 + t*0.437)*5.0;
y += sin(x*frequency*3.1122+ t*4.269)*2.5;
y *= amplitude*0.06;
// -----------------------------------------------------------------------------------------------------------------







// -----------------------------------------------------------------------------------------------------------------
vec2 st = gl_FragCoord.xy/u_resolution;
float pct = 0.0;
// a. The DISTANCE from the pixel to the center
pct = distance(st,vec2(vMouse));
// -----------------------------------------------------------------------------------------------------------------



// -----------------------------------------------------------------------------------------------------------------
float circular= circleShape(st,0.2, vMouse);
 vec3 col =vec3 (circular);
//invert Circel
col= vec3 (1.- col.x,1.- col.y,1.-col.z);
//get Image
 vec4 tex= texture2D(texture1,vUv);
//combine image and circle
 vec3 color= vec3(col.r* tex.r,col.g* tex.g, col.b* tex.b );
//combine image and circle
// -----------------------------------------------------------------------------------------------------------------

vec2 kurzwahl=st;
y=y*0.1;
float  pattern=step(sin(st.y+y*0.005)+tan(st.x/y)+cos(st.x/y),0.2);
//float  letssee=step(clamp(0.5,y,st.y+y) , 0.1);
float letssee = circleShape(vec2(st.x,st.y),0.2,vec2 (0.5, 0.3+y));
vec3 combined= vec3(letssee+ pattern);
combined= vec3 (1.-combined.r,1.- combined.g, 1.-combined.b);
pattern=step(sin(st.y/y*10.4),0.4);

if(color.r<=0.)
{
  color.r =color.r+ combined.r;
   color.g =color.g+ combined.g;
    color.b =color.g+ combined.b;
}



	gl_FragColor = vec4(color, 1.0 );
}
  `
}
var currentdate = new Date();
var imgtex = new THREE.TextureLoader();
var depthtex = new THREE.TextureLoader();

 let uniforms = {
   u_X:{type:'float', value: 1.0},
    u_Y:{type:'float', value: 1.0},
    u_resolution: { type: "v2", value: new THREE.Vector2()},
      texture1: {value:imgtex.load( 'Fancy.jpg' )},
       u_Time:{type:'float', value: 0.2},
    u_smoothedtome: {type:'float', value:0.4},
    }
 let custommaterial =  new THREE.ShaderMaterial({
    uniforms: uniforms,
    fragmentShader: fragmentShader(),
    vertexShader: vertexShader(),
    wireframe:false,
  })
//var material = new THREE.MeshBasicMaterial( {map: texture, side: THREE.DoubleSide} );
var plane = new THREE.Mesh( geometry, custommaterial);
      scene.add(plane );
  var realplane = new THREE.Mesh( geometry2, custommaterial);
      scene.add(realplane );    
       var object = scene.children[ 0 ];

     object.material.uniforms.u_resolution.value.x = window.innerWidth;
      object.material.uniforms.u_resolution.value.y = window.innerHeight*2;


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
    camera.position.z =window.innerHeight/2;
 var  secs=0;
var tl = anim.timeline({
  duration: 750,
  secs: secs++,
  easing: 'linear'
});



      function render() {

           renderer.render( scene, camera)
			}
      var objecttime=  object.material.uniforms.u_Time.value;
			var animate = function (time) {
       plane.rotation.x += 0.01;
			plane.rotation.y += 0.01;
        requestAnimationFrame( animate );
               time= Math.floor(time);
       // if(time%3===0){
       var tl = anim.timeline({
  duration:3000,
  secs: secs= secs+0.2,
  objecttime: object.material.uniforms.u_Time.value= secs ,
  easing: 'smooth'
});
         var wirreszeug=secs;
	    	render();
      };

      animate();
}}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

</style>
