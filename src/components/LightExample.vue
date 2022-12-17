<!-- @format -->

<style scoped>
.debugger {
	position: absolute;
	top: 0;
	left: 0;
	z-index: 99;
	color: white;
}
</style>

<template lang="pug">
Renderer(
	ref='renderer'
	shadow="" resize="" antialias="" pointer=""
	:orbit-ctrl='{ autoRotate: true, enableDamping: true, dampingFactor: 0.05 }' 
)

	Camera( :position='{ x: 0, y: 0, z: 13 }' )

	Scene(background='#000000')
		PointLight(
			ref='light'
			:intensity='.3'
			color="#f8f"
			:shadow-map-size='{ width: 512, height: 512 }'
			:position='{ x: 0, y: 0, z: 0 }'
		)
			Sphere(
				:radius='0.3'
			)

		Group( :rotation='{ x: -Math.PI / 2, y: 0, z: 0 }' )

			//=>> Square Lights
			RectAreaLight(
				color='#ff6000'
				:position='{ x: 0, y: 10, z: 3 }'
				:rotation='{ x: Math.PI * 1.75 }'
				v-bind='rectLightsProps'
				:intensity="15"
			)

			RectAreaLight(
				color='#ff00ff'
				:position='{ x: 10, y: 0, z: 3 }'
				:rotation='{ y: Math.PI * 2.25 }'
				v-bind='rectLightsProps'
				:intensity="15"
			)

			RectAreaLight(
				color='#ffcccc'
				:position='{ x: -10, y: 0, z: 3 }'
				:rotation='{ y: Math.PI * 1.75 }'
				v-bind='rectLightsProps'
				:intensity="15"
			)

			RectAreaLight(
				color='#0060ff'
				:position='{ x: 0, y: -10, z: 3 }'
				:rotation='{ x: Math.PI * 2.25 }'
				v-bind='rectLightsProps'
				:intensity="15"
			)

			//=>> Zonut
			GltfModel(
				ref='donut'
				src='/Donut.glb'
				:scale='{ x: 10, y: 10, z: 10 }'
				:rotation='{ x: 1.6, y: donut.rotateZ }'
				:position='{ z: -2.5 + donut.z }'
				@load='deboog'
			)

			Plane(
				:width='30'
				:height='30'
				:rotation='{ x: 0 }'
				:position='{ z: -3 }'
			)
				StandardMaterial(
					:props='{ displacementScale: 0.2, roughness: 0, metalness: 0 }'
				)
					Texture(
						:props='texturesProps'
						src='/textures/Wood_Tiles_002_basecolor.jpg'
					)
					Texture(
						:props='texturesProps'
						src='/textures/Wood_Tiles_002_hemight.png'
						name='displacementMap'
					)
					Texture(
						:props='texturesProps'
						src='/textures/Wood_Tiles_002_normal.jpg'
						name='normalMap'
					)
					Texture(
						:props='texturesProps'
						src='/textures/Wood_Tiles_002_roughness.jpg'
						name='roughnessMap'
					)
					Texture(
						:props='texturesProps'
						src='/textures/Wood_Tiles_002_ambientOcclusion.jpg'
						name='aoMap'
					)

	EffectComposer
		RenderPass
			UnrealBloomPass(:strength='0.3')
				FXAAPass


//- Teleport( to="#app" )
//- 	.debugger
//- 		h2 {{ donut.z }}
//- 		h2 {{ donut.dirUp }}
//- 		h2 {{ donut.rotateZ }}
</template>

<script lang="ts">
// textures from https://3dtextures.me/2019/04/26/wood-tiles-002/
import { RepeatWrapping } from 'three';
import {
	AmbientLight,
	Camera,
	EffectComposer,
	FXAAPass,
	Group,
	Renderer,
	GltfModel,
	Plane,
	PointLight,
	RectAreaLight,
	RenderPass,
	Scene,
	Sphere,
	StandardMaterial,
	Texture,
	UnrealBloomPass,
} from 'troisjs';

export default {
	components: {
		AmbientLight,
		Camera,
		EffectComposer,
		GltfModel,
		FXAAPass,
		Group,
		Renderer,
		Plane,
		PointLight,
		RectAreaLight,
		RenderPass,
		Scene,
		Sphere,
		StandardMaterial,
		Texture,
		UnrealBloomPass,
	},
	data() {
		return {
			texturesProps: {
				repeat: { x: 10, y: 10 },
				wrapS: RepeatWrapping,
				wrapT: RepeatWrapping,
			},
			rectLightsProps: {
				// rotation: { x: -Math.PI / 2 },
				lookAt: { x: 0, y: 0, z: 1 },
				intensity: 5,
				width: 5,
				height: 5,
				helper: true,
			},
			donut: {
				z: 0,
				rotateZ: 0,
				dirUp: true,
				anim: 0,
			},
		};
	},
	mounted() {
		const renderer = this.$refs.renderer;
		const light = this.$refs.light.light;
		const pointerV3 = renderer.three.pointer.positionV3;
		renderer.onBeforeRender(() => {
			light.position.copy(pointerV3);
		});
		// this.animate()
		let paused = false
		let inter = setInterval(() => this.animate(0, 1, 60), 16)

		// @ts-ignore
		document.addEventListener("keypress", (e: KeyboardEvent) => {
			console.log(this.donut.z, this.donut.dirUp, this.donut.anim)
			if (e.key == "v") {
				this.animate(0, 1, 60)
				return;
			}
			if (paused) {
				inter = setInterval(() => this.animate(0, 1, 60), 16)
			} else {
				clearInterval(inter)
			}
			paused = !paused
		})
	},
	methods: {
		animate(start, end, steps) {
			this.donut.rotateZ += .01;
			this.donut.anim++;
			const step = this.easeInOutQuad(this.donut.anim, start, end, steps);
			
			if (this.donut.dirUp) {
				this.donut.dirUp = this.donut.z < end;
				this.donut.z = step
			} else {
				this.donut.z = Math.abs(end-step);
				this.donut.dirUp = !this.donut.z > start;
			}
			this.donut.anim = this.donut.anim > steps ? 0 : this.donut.anim;
		},
		deboog() {
			console.log('Im here!');
		},
		/** AnimeJS is made for this but I'll try to make it myself first */
		easeInOutQuad(current, start, change, duration) {
			current = current <= 0 ? 1 : current 
			// current = current > duration ? duration : current

			current /= duration / 2;
			if (current < 1) return change / 2 * current * current + start;
			current--;
			return -change / 2 * (current * (current - 2) - 1) + start;

		},
		// bezierCurve(t, p0, p1, p2, p3): { x: number; y: number } {
		// 	let
		// 		cX = 3 * (p1.x - p0.x),
		// 		bX = 3 * (p2.x - p1.x) - cX,
		// 		aX = p3.x - p0.x - cX - bX,

		// 		cY = 3 * (p1.y - p0.y),
		// 		bY = 3 * (p2.y - p1.y) - cY,
		// 		aY = p3.y - p0.y - cY - bY,
				
		// 		x = (aX * Math.pow(t, 3)) + (bX * Math.pow(t, 2)) + (cX * t) + p0.x,
		// 		y = (aY * Math.pow(t, 3)) + (bY * Math.pow(t, 2)) + (cY * t) + p0.y;

		// 	return {x: x, y: y};
		// },

	},
};
</script>
