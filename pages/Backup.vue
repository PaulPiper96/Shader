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

import MyImage from '~/assets/ole dicke cam2.jpg'

export default {
mounted(){
var x;
var y;


  var scene = new THREE.Scene();
			var camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
			var renderer = new THREE.WebGLRenderer();
			renderer.setSize(  window.innerWidth, window.innerHeight);
      document.body.appendChild( renderer.domElement );
   var controls = new OrbitControls( camera, renderer.domElement );
scene.background = new THREE.Color( 0xffffff );
console.log("controls",controls)
  var geometry = new  THREE.PlaneBufferGeometry( window.innerWidth, window.innerHeight);

function vertexShader() {
  return `
     varying vec2 vUv;
       uniform float x;
        uniform float y;
      uniform sampler2D depthtext;
    varying float randnumb;
    void main() {
        vUv = uv;


 vec4 tex= texture2D( depthtext,vUv);
vec3 pos = position;

        gl_Position =   projectionMatrix * 
                        modelViewMatrix * 
                        vec4(pos,1.0);
    }
  `
}

function fragmentShader(){



  return `
      varying vec2 vUv;
      uniform float x;
      uniform float y;
      uniform sampler2D texture1;
      uniform sampler2D depthtext;
      uniform vec2  screenSize;
        varying float randnumb;
      uniform float pixelratio;

vec2 mirrored(vec2 v) {
  vec2 m = mod(v,2.);
  return mix(m,2.0 - m, step(1.0 ,m));
}


      void main() {
   vec2 screen= pixelratio*gl_FragCoord.xy*0.0005828;
   screen.y=screen.y+200.1;
         vec4 tex= texture2D(  depthtext,screen);
         tex.r=tex.r*0.4;
    float y2 =y*0.000065;
   float x2 =x*0.000065;
      vec2 fake3d=vec2(screen.x+tex.r+x2,screen.y+tex.r+y2);
    gl_FragColor = texture2D(texture1,mirrored(fake3d));
   //  gl_FragColor= vec4(tex.r,tex.r,tex.r,1.0);
      }
  `
}
var currentdate = new Date();
var imgtex = new THREE.TextureLoader();
var depthtex = new THREE.TextureLoader();
//imgtex.load( 'ole dicke cam2.jpg' );
 let uniforms = {
        colorB: {type: 'vec2', value: new THREE.Color(0xACB6E5)},
        colorA: {type: 'vec3', value: new THREE.Color(0x74ebd5)},
        x: {type:'float', value: 1.0},
         y: {type:'float', value: 1.0},
       texture1: {value:imgtex.load( 'ball.jpg' )},
       depthtext: {value:depthtex.load( 'ball-map.jpg' )},
      screenSize: {value:[ window.innerWidth, window.innerHeight]},
      pixelratio: {value:window.devicePixelRatio},
    }
 let custommaterial =  new THREE.ShaderMaterial({
    uniforms: uniforms,
    fragmentShader: fragmentShader(),
    vertexShader: vertexShader(),
  })
//var material = new THREE.MeshBasicMaterial( {map: texture, side: THREE.DoubleSide} );
var plane = new THREE.Mesh( geometry, custommaterial);
			scene.add(plane );

var light = new THREE.AmbientLight( 0xf0f0f0 ); // soft white light
scene.add( light );
document.addEventListener('mousemove', e => {
  if(x!=event.clientX){
    x = event.clientX;     // Get the horizontal coordinate
 y = event.clientY;     // Get the vertical coordinate
   var object = scene.children[ 0 ];
  console.log("x has done? ", object.material.uniforms.y.value);
   object.material.uniforms.y.value= y;
     object.material.uniforms.x.value=x;
 }
});
    
    camera.position.z =window.innerHeight/2;

      function render() {
        renderer.render( scene, camera );
			}


			var animate = function () {
        requestAnimationFrame( animate );
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
