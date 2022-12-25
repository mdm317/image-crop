<script>
  import { beforeUpdate } from 'svelte'
  import CutterFrame from './CutterFrame.svelte'
  export let cutterNum
  export let canvasWidth
  export let imgElem
  let filename = ''
  let cutterObjList = []
  let cropCavasElemList = []
  beforeUpdate(() => {
    if (cutterNum > cutterObjList.length) {
      cutterObjList = cutterObjList.concat({
        top: 0,
        left: 0,
        width: 950,
        height: 950,
      })
      cropCavasElemList = cropCavasElemList.concat(null)
    }
  })
  const changeTopLeft = (id, left, top) => {
    cutterObjList[id].top = top
    cutterObjList[id].left = left
  }
  const setCenter = (id) => {
    cutterObjList[id].left = (canvasWidth - cutterObjList[id].width) / 2
  }
  function downloadImageByA(data, filename = 'untitled.png') {
    var a = document.createElement('a')
    a.href = data
    a.download = filename
    document.body.appendChild(a)
    a.click()
  }
  const downloadImage = () => {
    cutterObjList.forEach((cutterObj, i) => {
      const cropCanvas = cropCavasElemList[i]
      cropCanvas.width = cutterObj.width
      cropCanvas.height = cutterObj.height
      const cropCtx = cropCanvas.getContext('2d')
      cropCtx.drawImage(
        imgElem,
        cutterObj.left,
        cutterObj.top,
        cutterObj.width,
        cutterObj.height,
        0,
        0,
        cutterObj.width,
        cutterObj.height
      )
      var dataURL = cropCanvas.toDataURL('image/png')
      downloadImageByA(dataURL, filename)
    })
  }
</script>

<input bind:value={filename} placeholder="Type file name" />
<button on:click={downloadImage}>click to download</button>
<div class="right-side-bar">
  {#each cutterObjList as cutterObj, i}
    <div>
      <header>{i + 1} 번째 커터</header>
      <div>
        <span>width</span>
        <input bind:value={cutterObj.width} />
      </div>
      <div>
        <span>height</span>
        <input bind:value={cutterObj.height} />
      </div>
      <div>
        <span>left</span>
        <input bind:value={cutterObj.left} />
      </div>
      <div>
        <span>top</span>
        <input bind:value={cutterObj.top} />
      </div>
      <button on:click={() => setCenter(i)}>set center</button>
    </div>
  {/each}
</div>
{#each Array(cutterNum) as _, i}
  <CutterFrame id={i} cutterPosObj={cutterObjList[i]} {changeTopLeft} />
{/each}
{#each Array(cutterObjList.length) as _, i}
  <canvas bind:this={cropCavasElemList[i]} />
{/each}

<style>
  .right-side-bar {
    width: 30vw;
    position: fixed;
    top: 0px;
    right: 0px;
    background-color: wheat;
  }
  canvas {
    border: 1px solid red;
  }
</style>
