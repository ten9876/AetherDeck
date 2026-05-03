<script lang="ts">
	import { getCurrentWindow } from "@tauri-apps/api/window";

	const win = getCurrentWindow();

	let title = "";
	$: void (async () => {
		title = await win.title();
	})();

	function startDrag(e: MouseEvent) {
		if (e.button !== 0) return;
		const target = e.target as HTMLElement;
		// Don't start a drag if the click landed on a button (window controls)
		if (target.closest("button")) return;
		void win.startDragging();
	}

	function maybeMaximize(e: MouseEvent) {
		if (e.button !== 0) return;
		const target = e.target as HTMLElement;
		if (target.closest("button")) return;
		void win.toggleMaximize();
	}

	function minimize() {
		void win.minimize();
	}
	function toggleMaximize() {
		void win.toggleMaximize();
	}
	function close() {
		void win.close();
	}
</script>

<div
	class="flex flex-row items-center select-none bg-neutral-900 h-7 shrink-0"
	on:mousedown={startDrag}
	on:dblclick={maybeMaximize}
	role="toolbar"
	tabindex="-1"
>
	<div class="flex-1 px-3 text-xs text-neutral-400 pointer-events-none">
		{title}
	</div>
	<div class="flex flex-row items-center h-full">
		<button
			type="button"
			class="flex items-center justify-center w-11 h-full text-neutral-400 hover:bg-neutral-800 transition-colors cursor-pointer"
			aria-label="Minimize"
			on:click={minimize}
		>
			<svg width="10" height="10" viewBox="0 0 10 10" fill="none" xmlns="http://www.w3.org/2000/svg">
				<line x1="0" y1="5" x2="10" y2="5" stroke="currentColor" stroke-width="1" />
			</svg>
		</button>
		<button
			type="button"
			class="flex items-center justify-center w-11 h-full text-neutral-400 hover:bg-neutral-800 transition-colors cursor-pointer"
			aria-label="Maximize"
			on:click={toggleMaximize}
		>
			<svg width="10" height="10" viewBox="0 0 10 10" fill="none" xmlns="http://www.w3.org/2000/svg">
				<rect x="0.5" y="0.5" width="9" height="9" stroke="currentColor" stroke-width="1" fill="none" />
			</svg>
		</button>
		<button
			type="button"
			class="flex items-center justify-center w-11 h-full text-neutral-400 hover:bg-red-600 hover:text-white transition-colors cursor-pointer"
			aria-label="Close"
			on:click={close}
		>
			<svg width="10" height="10" viewBox="0 0 10 10" fill="none" xmlns="http://www.w3.org/2000/svg">
				<line x1="0" y1="0" x2="10" y2="10" stroke="currentColor" stroke-width="1" />
				<line x1="0" y1="10" x2="10" y2="0" stroke="currentColor" stroke-width="1" />
			</svg>
		</button>
	</div>
</div>
