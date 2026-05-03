<script lang="ts">
	import { getWebserverUrl, getWebSocketPort } from "$lib/ports";

	import Popup from "./Popup.svelte";

	import { invoke } from "@tauri-apps/api/core";

	export let plugin: { id: string; name: string; property_inspector_path?: string | null } | null = null;

	let iframe: HTMLIFrameElement | null = null;
	let resolvedSrc = "";

	$: show = plugin !== null && !!plugin.property_inspector_path;

	$: if (plugin && plugin.property_inspector_path) {
		const target = plugin;
		void (async () => {
			const absPath = await invoke<string>("get_plugin_property_inspector_path", { plugin: target.id });
			// Guard against rapid plugin switches.
			if (plugin === target) {
				resolvedSrc = getWebserverUrl(absPath + "|opendeck_property_inspector");
			}
		})();
	} else {
		resolvedSrc = "";
	}

	async function onIframeLoad() {
		if (!plugin || !iframe || !iframe.src.startsWith(getWebserverUrl())) return;
		const info = JSON.stringify(await invoke("make_info", { plugin: plugin.id }));
		// Plugin-level PI registers with the plugin UUID as context, so
		// setGlobalSettings/getGlobalSettings calls route to the correct plugin.
		iframe.contentWindow?.postMessage({
			event: "connect",
			payload: [
				getWebSocketPort(),
				plugin.id,
				"registerPropertyInspector",
				info,
			],
		}, getWebserverUrl());
	}

	function close() {
		plugin = null;
	}
</script>

<Popup {show} label="Plugin Settings">
	{#if plugin}
		<div class="flex flex-row items-center justify-between mb-3">
			<h2 class="text-lg font-semibold text-neutral-200">{plugin.name} — Plugin Settings</h2>
			<button
				class="text-2xl text-neutral-300 hover:text-white"
				on:click={close}
				aria-label="Close"
			>
				✕
			</button>
		</div>
		{#if resolvedSrc}
			<iframe
				bind:this={iframe}
				title="Plugin Settings"
				class="w-full h-[calc(100%-3rem)] bg-neutral-900 border border-neutral-700 rounded"
				src={resolvedSrc}
				on:load={onIframeLoad}
			/>
		{:else}
			<p class="text-neutral-400 text-sm">Loading…</p>
		{/if}
	{/if}
</Popup>
