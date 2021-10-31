

<!-- <script context="module">
	import { browser, dev } from '$app/env';
	import { onMount } from 'svelte';
	import * as THREE from "three"; 
	
	import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.cjs'; // NEED TO FIX THIS IN NETLIFY SOMEHOW. 
	import { FontLoader } from 'three/examples/jsm/loaders/FontLoader.cjs'; 
	
	export const prerender = false;
</script>

<script lang="ts">
	
	function animate () {
		requestAnimationFrame( animate ); 
		
		torus.rotation.x += 0.01; 
		torus.rotation.y += 0.005; 
		torus.rotation.z += 0.01; 

		controls.update(); 

		renderer.render(scene, camera); 
	}

	function addStar() {
		const starGeometry = new THREE.SphereGeometry(0.25, 24, 24);
		const starMaterial = new THREE.MeshStandardMaterial({ color: 0xffffff });
		const star = new THREE.Mesh( starGeometry, starMaterial ); 

		const [ x, y, z ] = Array(3).fill(1).map(() => THREE.MathUtils.randFloatSpread( 100 )); 
		star.position.set(x, y, z); 

		scene.add(star); 
	}

	let canvas; 
	let scene;
	let camera;
	let renderer;
	
	let geometry; 
	let material; 
	let torus; 
	let pointLight;
	let pointLightHelper;
	let ambientLight;
	
	let gridHelper; 
	let controls; 
	
	let moon; 

	onMount(() => {
		
		function moveCamera() {
			console.log('firing scroll');
			const t = document.body.getBoundingClientRect().top; 
			moon.rotation.x += 0.05; 
			moon.rotation.y += 0.075; 
			moon.rotation.z += 0.05; 

			camera.position.x * -0.01;
			camera.position.y * -0.002;
			camera.position.z * -0.002;
		}

		// document.body.onscroll = moveCamera; 

		scene = new THREE.Scene();
		camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 ); 
		renderer = new THREE.WebGLRenderer({
			canvas: document.querySelector("#bg")
		})

		renderer.setPixelRatio(window.devicePixelRatio);
		renderer.setSize( window.innerWidth, window.innerHeight);
		camera.position.setZ(30); 
		// camera.position.setX(-3);

		geometry = new THREE.TorusGeometry( 10, 3, 16, 100 ); 
		material = new THREE.MeshStandardMaterial({ color: 0xFF6347, wireframe: false })
		torus = new THREE.Mesh( geometry, material ); 
		scene.add(torus);

		Array(200).fill(1).forEach(addStar); 

		const fontLoader = new FontLoader();
		
		fontLoader.load(
			"/helvetiker_regular.typeface.json",
			// onLoad callback
			( font ) => {
				// do something with the font
				console.log( font );

				const color = 0x006699;

				const matDark = new THREE.LineBasicMaterial( {
					color: color,
					side: THREE.DoubleSide
				} );

				const matLite = new THREE.MeshBasicMaterial( {
					color: color,
					transparent: true,
					opacity: 0.4,
					side: THREE.DoubleSide
				} );

				const message = "i io ioo";

				const shapes = font.generateShapes( message, 100 );

				const geometry = new THREE.ShapeGeometry( shapes );

				geometry.computeBoundingBox();

				const xMid = - 0.5 * ( geometry.boundingBox.max.x - geometry.boundingBox.min.x );

				geometry.translate( xMid, 0, 0 );

				// make shape ( N.B. edge view not visible )

				const text = new THREE.Mesh( geometry, matLite );
				text.position.z = - 150;
				scene.add( text );

				// make line shape ( N.B. edge view remains visible )

				const holeShapes = [];

				for ( let i = 0; i < shapes.length; i ++ ) {

					const shape = shapes[ i ];

					if ( shape.holes && shape.holes.length > 0 ) {

						for ( let j = 0; j < shape.holes.length; j ++ ) {

							const hole = shape.holes[ j ];
							holeShapes.push( hole );

						}

					}

				}

				shapes.push.apply( shapes, holeShapes );

				const lineText = new THREE.Object3D();

				for ( let i = 0; i < shapes.length; i ++ ) {

					const shape = shapes[ i ];

					const points = shape.getPoints();
					const geometry = new THREE.BufferGeometry().setFromPoints( points );

					geometry.translate( xMid, 0, 0 );

					const lineMesh = new THREE.Line( geometry, matDark );
					lineText.add( lineMesh );

				}

				scene.add( lineText );
			},

			// onProgress callback
			( xhr ) =>  {
				console.log( (xhr.loaded / xhr.total * 100) + '% loaded' );
			},

			// onError callback
			( err ) =>  {
				console.log( 'An error happened: ', err );
			}
		);

		
		pointLight = new THREE.PointLight(0xffffff);
		pointLight.position.set(5, 5, 5); 
		pointLightHelper = new THREE.PointLightHelper(pointLight);
		ambientLight = new THREE.AmbientLight(0xffffff)

		gridHelper = new THREE.GridHelper(200, 50);
		controls = new OrbitControls(camera, renderer.domElement); 

		scene.add(pointLight, pointLightHelper, ambientLight, gridHelper);

		let moonTexture = new THREE.TextureLoader().load('http://localhost:3000/moon.jpeg')
		let normalTexture = new THREE.TextureLoader().load('http://localhost:3000/normal.jpeg')
		moon = new THREE.Mesh(
			new THREE.SphereGeometry(3, 32, 32),
			new THREE.MeshStandardMaterial({
				map: moonTexture,
				normalMap: normalTexture
			})
		)
		moon.position.y = 30;

		scene.add(moon);

		animate(); 
	
	})

</script>

<svelte:head>
	<title>About</title>
</svelte:head>


<div class="container">
	<canvas id="bg"/>
</div>

<style>
	canvas {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}

	.container {
		position: absolute;
		width: 100%;
		height: 100%;
	}

	p {
		color: white;
	}
</style> -->


<script lang="ts">
import { onMount } from "svelte";

	let inputElement = document.getElementById("input");

	function handleFiles(evt) {
		
		const file = evt.target.files[0]; 
		
		let reader = new FileReader();
        reader.onload = (function(reader)
        {
            return function()
            {
                let contents = reader.result as string;
				// console.log("CONTENTS: ", contents);
                let lines = contents.split('==========').map( line => line.split("\r\n").filter(v => !!v) );
                // console.log("LINES: ", lines); 
				const objs = lines.reduce((acc, curr) => {
					// console.log("CURR: ", curr); 
					if(!curr.length) {
						return acc; 
					}
					const author = curr[0].match(/\((.*?)\)/)[1];
					const title = curr[0].split(`(${author})`)[0].trim();
					const type = curr[1].includes("Note") ? "note" : "highlight"; 
					const content = curr[2]; 
					return [ ...acc, { title, author, type, content } ]; 
				}, []);
                //////
                console.log("OBJS: ", objs); 
            }
        })(reader);

        reader.readAsText(file);

	}
	
	onMount(() => {
	
	})
	
</script>

<input type="file" id="input" on:change={(evt) => handleFiles(evt)} multiple>