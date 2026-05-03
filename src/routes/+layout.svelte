<script>
	import "../app.css";
	import Titlebar from "../components/Titlebar.svelte";
	import ResizeHandles from "../components/ResizeHandles.svelte";
</script>

<div class="flex flex-col h-screen w-screen overflow-hidden">
	<Titlebar />
	<div class="flex-1 min-h-0">
		<slot />
	</div>
</div>
<ResizeHandles />
