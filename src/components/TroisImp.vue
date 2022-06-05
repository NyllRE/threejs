<script setup>
import { Camera, Renderer, Scene, Box, Sphere, Plane, Texture, BasicMaterial, PhongMaterial, LambertMaterial, PointLight, AmbientLight, GltfModel } from 'troisjs'
import { onMounted, ref } from 'vue'

//Load background texture

let renderer = ref()
let box = ref()
let sphere = ref()
let camera = ref()


const pull = ( from, to, power ) => {
	if (from <= to) {
		from += Math.hypot(from - to) * power
	} else {
		from -= Math.hypot(from - to) * power
	}
	return from
}


onMounted(() => {
	box = box.value.mesh
	sphere = sphere.value.mesh
	camera = camera.value.camera


	renderer = renderer.value
	renderer.onBeforeRender(() => {
		box.rotation.y += .01
		sphere.rotation.y += .002

		camera.position.z = pull(camera.position.z, 15, .004)
		camera.position.y = pull( camera.position.y, 0, .01 )
	})

})

</script>


<template lang="pug">
Renderer( ref="renderer" resize="window" orbitCtrl antialias alpha shadow )
	Camera( ref="camera" :position="{ x:3,z:-35 }" )

	Scene( ref="scene" )
		PointLight( ref="light" :position="{ x:0,y:30,z:-8 }" color="#fff" castShadow intensity="1" :shadow-map-size="{ width: 512, height: 512 }" )


		Box( ref="box" :position="{ y:-10 }" :size="2" castShadow )
			LambertMaterial( color="#222" )
				//Texture( color="#222" )
					

		Sphere( ref="sphere" :position="{ x:0, y:0, z:-200 }" :radius="80" recieveShadow="true" )
			PhongMaterial
				Texture( src="/mars" )


		//Plane( ref="plane" :position="{ y:-10 }" recieve-shadow :scale="{ x:10, y:10 }" :rotation="{ x:-1.5 }" )
			PhongMaterial( color="#000" )

		GltfModel( ref="abojaber" src="/abojaber.glb" :scale="{ x:10,y:10,z:10 }" :position="{ y:-4.5 }" :rotation="{ y: 1.3 }" )


</template>


<style lang="sass">
</style>
