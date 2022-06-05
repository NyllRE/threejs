<style lang="sass" scoped>

canvas
	height: 100%
	width: 100%
	background: black

</style>


<template lang="pug">

canvas(ref="canvas")

</template>

<script setup>
import * as T3 from 'three'
import { onMounted, ref } from 'vue'

const canvas = ref(null)

onMounted( () => {
	const scene = new T3.Scene()
	const renderer = new T3.WebGLRenderer({ canvas: canvas.value, alpha: true })
	renderer.setSize(innerWidth, innerHeight)
	renderer.setPixelRatio(devicePixelRatio)


	// degree, aspect-ratio, clip out of screen, max distance render 
	const camera = new T3.PerspectiveCamera(75, innerWidth / innerHeight, 0.1, 1000)
/*
	const light = new T3.PointLight( 0xffffff, 2 )
	light.position.set(0, 0, 2)
	light.intensity = 1.7

	scene.add(light)
*/
	// Plane
	const geometry = new T3.PlaneGeometry( 5,5,10,10 );
	const material = new T3.MeshStandardMaterial()
	material.metalness = 0.85
	material.roughness = 0


	//gui.add(material, 'metalness').min(0).max(1)
	//gui.add(material, 'roughness').min(0).max(1)


	material.color = new T3.Color(0xffffff)
	const plane = new T3.Mesh( geometry, material );
	scene.add( plane );


	function animate() {
		renderer.render( scene, camera )

		// plesh.rotation.y += .01

		requestAnimationFrame(animate)
	}
	animate()

})

</script>
