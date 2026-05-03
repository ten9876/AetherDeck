<script lang="ts">
	import { getCurrentWindow } from "@tauri-apps/api/window";

	const win = getCurrentWindow();

	type ResizeDir = "North" | "NorthEast" | "East" | "SouthEast" | "South" | "SouthWest" | "West" | "NorthWest";

	function start(direction: ResizeDir) {
		return (e: MouseEvent) => {
			if (e.button !== 0) return;
			e.preventDefault();
			e.stopPropagation();
			void win.startResizeDragging(direction);
		};
	}
</script>

<!-- Edges (4px hit zones) -->
<div class="resize-handle resize-n cursor-n-resize" on:mousedown={start("North")} role="presentation"></div>
<div class="resize-handle resize-s cursor-s-resize" on:mousedown={start("South")} role="presentation"></div>
<div class="resize-handle resize-e cursor-e-resize" on:mousedown={start("East")} role="presentation"></div>
<div class="resize-handle resize-w cursor-w-resize" on:mousedown={start("West")} role="presentation"></div>
<!-- Corners (8px hit zones, sit on top of edges) -->
<div class="resize-handle resize-ne cursor-ne-resize" on:mousedown={start("NorthEast")} role="presentation"></div>
<div class="resize-handle resize-nw cursor-nw-resize" on:mousedown={start("NorthWest")} role="presentation"></div>
<div class="resize-handle resize-se cursor-se-resize" on:mousedown={start("SouthEast")} role="presentation"></div>
<div class="resize-handle resize-sw cursor-sw-resize" on:mousedown={start("SouthWest")} role="presentation"></div>

<style>
	.resize-handle {
		position: fixed;
		z-index: 100;
	}
	.resize-n { top: 0; left: 8px; right: 8px; height: 4px; }
	.resize-s { bottom: 0; left: 8px; right: 8px; height: 4px; }
	.resize-e { top: 8px; right: 0; bottom: 8px; width: 4px; }
	.resize-w { top: 8px; left: 0; bottom: 8px; width: 4px; }
	.resize-ne { top: 0; right: 0; width: 8px; height: 8px; }
	.resize-nw { top: 0; left: 0; width: 8px; height: 8px; }
	.resize-se { bottom: 0; right: 0; width: 8px; height: 8px; }
	.resize-sw { bottom: 0; left: 0; width: 8px; height: 8px; }
</style>
