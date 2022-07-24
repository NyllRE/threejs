<!-- <template>
  <Renderer ref="renderer" shadow resize antialias :orbit-ctrl="{ autoRotate: true, enableDamping: true, dampingFactor: 0.05 }" pointer>
    <Camera :position="{ x: 0, y: 0, z: 10 }" />
    <Scene background="#000000" >
      <PointLight cast-shadow :shadow-map-size="{ width: 512, height: 512 }" ref="light" :intensity="0.7" :position="{ x: 0, y: 0, z: 0 }">
        <Sphere :radius="0.3" recieve-shadow />
      </PointLight>
      <Group :rotation="{ x: -Math.PI / 2, y: 0, z: 0 }">
        <RectAreaLight cast-shadow
          color="#ff6000"
          :position="{ x: 0, y: 10, z: 1 }"
          v-bind="rectLightsProps"
        />
        <RectAreaLight cast-shadow
          color="#0060ff"
          :position="{ x: 10, y: 0, z: 1 }"
          v-bind="rectLightsProps"
        />
        <RectAreaLight cast-shadow
          color="#60ff60"
          :position="{ x: -10, y: 0, z: 1 }"
          v-bind="rectLightsProps"
        />
        <RectAreaLight cast-shadow
          color="#ffffff"
          :position="{ x: 0, y: -10, z: 1 }"
          v-bind="rectLightsProps"
        />
    		<GltfModel cast-shadow
          ref="donut"
          src="/Donut.glb"
          :scale="{ x:10,y:10,z:10 }"
          :rotation="{ x: 1.6 }"
          :position="{ z:-2.5 }"
          @load="deboog"
        />

        <Plane
          recieve-shadow
          :width="30"
          :height="30"
          :rotation="{ x: 0 }"
          :position="{ z: -3 }"
        >
          <StandardMaterial
            :props="{ displacementScale: 0.2, roughness: 0, metalness: 0 }"
          >
            <Texture
              :props="texturesProps"
              src="/textures/Wood_Tiles_002_basecolor.jpg"
            />
            <Texture
              :props="texturesProps"
              src="/textures/Wood_Tiles_002_height.png" name="displacementMap"
            />
            <Texture
              :props="texturesProps"
              src="/textures/Wood_Tiles_002_normal.jpg"
              name="normalMap"
            />
            <Texture
              :props="texturesProps"
              src="/textures/Wood_Tiles_002_roughness.jpg"
              name="roughnessMap"
            />
            <Texture
              :props="texturesProps"
              src="/textures/Wood_Tiles_002_ambientOcclusion.jpg"
              name="aoMap"
            />
          </StandardMaterial>
        </Plane>
      </Group>
    </Scene>
    <EffectComposer>
      <RenderPass />
      <UnrealBloomPass :strength="0.3" />
      <FXAAPass />
    </EffectComposer>
  </Renderer>
</template> -->

<template lang="pug">
Renderer(
  ref='renderer'
  shadow resize antialias pointer
  :orbit-ctrl='{ autoRotate: true, enableDamping: true, dampingFactor: 0.05 }' 
)

  Camera( :position='{ x: 0, y: 0, z: 10 }' )

  Scene(background='#000000')
    PointLight(
      ref='light'
      cast-shadow
      :intensity='0.7'
      :shadow-map-size='{ width: 512, height: 512 }'
      :position='{ x: 0, y: 0, z: 0 }'
    )
      Sphere(
        :radius='0.3'
        recieve-shadow
      )

    Group( :rotation='{ x: -Math.PI / 2, y: 0, z: 0 }' )

      RectAreaLight(
        cast-shadow=''
        color='#ff6000'
        :position='{ x: 0, y: 10, z: 1 }'
        v-bind='rectLightsProps'
      )

      RectAreaLight(
        cast-shadow
        color='#0060ff'
        :position='{ x: 10, y: 0, z: 1 }'
        v-bind='rectLightsProps'
      )

      RectAreaLight(
        cast-shadow
        color='#60ff60'
        :position='{ x: -10, y: 0, z: 1 }'
        v-bind='rectLightsProps'
      )

      RectAreaLight(
        cast-shadow
        color='#ffffff'
        :position='{ x: 0, y: -10, z: 1 }'
        v-bind='rectLightsProps'
      )

      GltfModel(
        cast-shadow
        ref='donut'
        src='/Donut.glb'
        :scale='{ x:10,y:10,z:10 }'
        :rotation='{ x: 1.6 }'
        :position='{ z:-2.5 }'
        @load='deboog'
      )

      Plane(
        recieve-shadow
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
            src='/textures/Wood_Tiles_002_height.png'
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

</template>

<script>
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
    };
  },
  mounted() {
    const renderer = this.$refs.renderer;
    const light = this.$refs.light.light;
    const pointerV3 = renderer.three.pointer.positionV3;
    renderer.onBeforeRender(() => {
      light.position.copy(pointerV3);
    });
  },
  methods: {
    deboog() {
      console.log('Im here!')
    }
  }
};
</script>
