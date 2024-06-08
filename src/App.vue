<script setup lang="ts">
import invariant from 'tiny-invariant'
import { ref, onMounted, onUnmounted } from 'vue'

const canvas = ref<HTMLCanvasElement | null>(null)

function getMousePos(event: MouseEvent) {
  return { x: event.offsetX, y: event.offsetY }
}

function getCanvasContext() {
  const ctx = canvas.value?.getContext('2d')
  invariant(ctx, 'Canvas ctx is not defined')
  return ctx
}

function handleMouseDown(event: MouseEvent) {
  const ctx = getCanvasContext()
  ctx.beginPath()

  // styles for the line
  ctx.lineWidth = 5
  ctx.lineCap = 'round'
  ctx.strokeStyle = '#000'

  const pos = getMousePos(event)
  ctx.moveTo(pos.x, pos.y)
}

function handleMouseMove(event: MouseEvent) {
  const pos = getMousePos(event)
  const ctx = getCanvasContext()
  ctx.lineTo(pos.x, pos.y)
  ctx.stroke()
}

function handleMouseUp() {
  const ctx = getCanvasContext()
  ctx.closePath()
}

onMounted(() => {
  if (!canvas.value) return
  canvas.value.addEventListener('mousedown', handleMouseDown)
  canvas.value.addEventListener('mousemove', handleMouseMove)
  canvas.value.addEventListener('mouseup', handleMouseUp)
})

onUnmounted(() => {
  if (!canvas.value) return
  canvas.value.removeEventListener('mousedown', handleMouseDown)
  canvas.value.removeEventListener('mousemove', handleMouseMove)
  canvas.value.removeEventListener('mouseup', handleMouseUp)
})
</script>

<template>
  <main class="flex min-h-dvh flex-col items-center justify-center gap-8 bg-gray-900 text-gray-50">
    <h1 class="text-3xl font-bold">Drawing board</h1>
    <canvas ref="canvas" class="rounded-lg bg-gray-200" width="800" height="600" />
  </main>
</template>
