<script>
import CanvasUtil from "./CanvasUtil.svelte";
import CutterFrame from "./CutterFrame.svelte";

    let imgElem
    let canvasElem
    let cutterNum=0
    let canvasWidth
    let canvasHeight
    let sourceImage
    const onImageUpload = (e)=>{
        var URL = window.URL;
        var url = URL.createObjectURL(e.target.files[0]);
        sourceImage = url
        imgElem.src = url
    }
    const onLoad = (e)=>{
        const height = e.target.naturalHeight
        const width = e.target.naturalWidth
        if(canvasElem){
            canvasElem.width = width
            canvasElem.height = height
            canvasWidth = width
            canvasHeight = height
            var ctx = canvasElem.getContext("2d");        
            ctx.clearRect(0, 0,width, height);
            ctx.drawImage(e.target, 0, 0, width, height);
            cutterNum =1
        }
    }
 
</script>
<span>upload image</span>
<input  on:change={onImageUpload}  type='file' accept="image/*"/>
<img  bind:this={imgElem} on:load={onLoad} class="hidden-image" alt='hidden'>
<input bind:value={cutterNum} type="number">
<div style="width:{canvasWidth}px;height:{canvasHeight}" class="canvas-wrapper">
    <canvas style="border: 1px solid black;" bind:this={canvasElem} >
    </canvas>
    <CanvasUtil imgElem={imgElem} canvasWidth={canvasWidth} cutterNum ={cutterNum}/>
</div>

<style>
    :global(body) {
		/* this will apply to <body> */
            background-color: beige;
	}
    .hidden-image{
        display: none;
    }
    .canvas-wrapper{
        position: relative;

    }
</style>