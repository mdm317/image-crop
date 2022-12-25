<script>
  export let cutterPosObj
  export let id
  export let changeTopLeft
  import { beforeUpdate } from 'svelte'
  let st = `width: ${cutterPosObj.width}px;height:${cutterPosObj.height}px;top:${cutterPosObj.top}px;left:${cutterPosObj.left}px;`
  let isMouseDown = false
  let mouseDownX = 0
  let mouseDownY = 0
  beforeUpdate(() => {
    st = `width: ${cutterPosObj.width}px;height:${cutterPosObj.height}px;top:${cutterPosObj.top}px;left:${cutterPosObj.left}px;`
  })

  function mouseDown(e) {
    if (e.buttons === 1) {
      isMouseDown = true
      mouseDownX = e.clientX
      mouseDownY = e.clientY
    }
  }
  function mouseMove(e) {
    if (isMouseDown) {
      const moveX = mouseDownX - e.clientX
      const moveY = mouseDownY - e.clientY
      mouseDownX = e.clientX
      mouseDownY = e.clientY
      changeTopLeft(id, cutterPosObj.left - moveX, cutterPosObj.top - moveY)
      st = `width: ${cutterPosObj.width}px;height:${cutterPosObj.height}px;top:${cutterPosObj.top}px;left:${cutterPosObj.left}px;`
    }
  }
  function mouseUp(e) {
    console.log(id)
    isMouseDown = false
  }
</script>

<svelte:window on:mousemove={mouseMove} />
<div style={st} on:mousedown={mouseDown} on:mouseup={mouseUp} class={`wrapper ${isMouseDown ? 'upper' : ''}`} />

<style>
  .wrapper {
    position: absolute;
    border: 1px solid blue;
  }
  .upper {
    z-index: 10;
  }
</style>
