<script>
	import Moveable from "svelte-moveable";
	let target;

	// Event handlers for drag events
	function onDragStart(detail) {
		console.log("Drag Start:", detail);
	}

	function onDrag(detail) {
		console.log("Dragging:", detail);
	}

	function onDragEnd(detail) {
		console.log("Drag End:", detail);
	}

	// Event handlers for resize events
	function onResizeStart(detail) {
		console.log("Resize Start:", detail);
	}

	function onResize(detail) {
		console.log("Resizing:", detail);
	}

	function onResizeEnd(detail) {
		console.log("Resize End:", detail);
	}

	// Event handlers for scale events
	function onScaleStart(detail) {
		console.log("Scale Start:", detail);
	}

	function onScale(detail) {
		console.log("Scaling:", detail);
	}

	function onScaleEnd(detail) {
		console.log("Scale End:", detail);
	}

	// Event handlers for rotate events
	function onRotateStart(detail) {
		console.log("Rotate Start:", detail);
	}

	function onRotate(detail) {
		console.log("Rotating:", detail);
	}

	function onRotateEnd(detail) {
		console.log("Rotate End:", detail);
	}

	// Event handlers for warp events
	function onWarpStart(detail) {
		console.log("Warp Start:", detail);
	}

	function onWarp(detail) {
		console.log("Warping:", detail);
	}

	function onWarpEnd(detail) {
		console.log("Warp End:", detail);
	}

	// Event handlers for pinch events
	function onPinchStart(detail) {
		console.log("Pinch Start:", detail);
	}

	function onPinch(detail) {
		console.log("Pinching:", detail);
	}

	function onPinchEnd(detail) {
		console.log("Pinch End:", detail);
	}
</script>

<style>
	.target {
		width: 100px;
		height: 100px;
		background-color: #f00;
	}
</style>

<div class="target" bind:this={target}>Target</div>
<Moveable
	target={target}
	origin={true}
	edge={false}

	draggable={true}
	throttleDrag={0}
	on:dragStart={({ detail }) => onDragStart(detail)}
	on:drag={({ detail }) => onDrag(detail)}
	on:dragEnd={({ detail }) => onDragEnd(detail)}

	keepRatio={false}
	renderDirections={["nw", "ne", "sw", "se", "n", "w", "s", "e"]}

	resizable={false}
	throttleResize={0}
	on:resizeStart={({ detail }) => onResizeStart(detail)}
	on:resize={({ detail }) => onResize(detail)}
	on:resizeEnd={({ detail }) => onResizeEnd(detail)}

	scalable={false}
	throttleScale={0}
	on:scaleStart={({ detail }) => onScaleStart(detail)}
	on:scale={({ detail }) => onScale(detail)}
	on:scaleEnd={({ detail }) => onScaleEnd(detail)}

	rotatable={false}
	throttleRotate={0}
	on:rotateStart={({ detail }) => onRotateStart(detail)}
	on:rotate={({ detail }) => onRotate(detail)}
	on:rotateEnd={({ detail }) => onRotateEnd(detail)}

	warpable={false}
	on:warpStart={({ detail }) => onWarpStart(detail)}
	on:warp={({ detail }) => onWarp(detail)}
	on:warpEnd={({ detail }) => onWarpEnd(detail)}

	pinchable={false}
	on:pinchStart={({ detail }) => onPinchStart(detail)}
	on:pinch={({ detail }) => onPinch(detail)}
	on:pinchEnd={({ detail }) => onPinchEnd(detail)}
/>
