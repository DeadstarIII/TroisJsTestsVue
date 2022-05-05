<template>
  <InstancedMesh ref="imesh" :count="500">
    <SphereGeometry :radius="5" />
    <PhongMaterial color="#ffffff" />
  </InstancedMesh>
</template>

<script setup>
import { ref, onMounted } from "vue";
import {
  Box,
  Camera,
  LambertMaterial,
  PointLight,
  Renderer,
  Scene,
} from "troisjs";
const imesh = this.$refs.imesh.mesh;
const dummy = new Object3D();
const { randFloat: rnd, randFloatSpread: rndFS } = MathUtils;
for (let i = 0; i < 500; i++) {
  dummy.position.set(rndFS(200), rndFS(200), rndFS(200));
  const scale = rnd(0.2, 1);
  dummy.scale.set(scale, scale, scale);
  dummy.updateMatrix();
  imesh.setMatrixAt(i, dummy.matrix);
}
imesh.instanceMatrix.needsUpdate = true;
onMounted(() => {
  const renderer = rendererC.value;
  const mesh = meshC.value.mesh;
  renderer.onBeforeRender(() => {
    mesh.rotation.x += 0.01;
  });
});
</script>

<style>
body {
  margin: 0;
}
canvas {
  display: block;
}
</style>
