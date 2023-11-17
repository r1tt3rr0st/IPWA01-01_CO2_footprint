<script lang="ts">
	import '../app.postcss';
	import { AppShell, AppBar } from '@skeletonlabs/skeleton';
	import { initializeStores, Drawer, getDrawerStore, LightSwitch } from '@skeletonlabs/skeleton';
	import Navigation from '$lib/Navigation/Navigation.svelte';
	import { onMount } from "svelte";

	let isrtl = false;

	onMount(() => {
	const language = navigator.language;
	const countryCode = language.split("-")[0];
	isrtl = ["ar", "he"].includes(countryCode);
	});

	initializeStores();
	const drawerStore = getDrawerStore();

	function drawerOpen(): void {
		if (isrtl === false) {
			drawerStore.open({position: "left"});
		} else {
			drawerStore.open({position: "right"});
		}
		
	}
</script>

<Drawer>
	{#if !isrtl}
		<h2 class="p-4">Navigation</h2>
	{/if}
	{#if isrtl}
		<h2 dir="rtl" class="p-4">Navigation</h2>
	{/if}
	<hr />
	<Navigation />
</Drawer>

<AppShell slotSidebarLeft="bg-surface-500/5 w-56 p-4">
	<svelte:fragment slot="header">
		<AppBar>
			<svelte:fragment slot="lead">
				{#if isrtl}
					<a
						class="btn btn-sm variant-ghost-surface"
						href="https://github.com/r1tt3rr0st/IPWA01-CO2_footprint"
						target="_blank"
						rel="noreferrer"
					>
						GitHub
					</a>
				{/if}
				{#if !isrtl}
					<div class="flex items-center space-x-2">
						<button class=" btn btn-sm mr-4" on:click={drawerOpen}>
							<span>
								<svg viewBox="0 0 100 80" class="fill-token w-4 h-4">
									<rect width="100" height="20" />
									<rect y="30" width="100" height="20" />
									<rect y="60" width="100" height="20" />
								</svg>
							</span>
						</button>
						<a href="/">
							<strong class="text-xl uppercase">CO<sub>2</sub>-Footprint</strong>
						</a>
						<a href="/">
							<img src="co2.png" alt="CO2 Icon">
						</a>
					</div>
				{/if}
			</svelte:fragment>

			<svelte:fragment slot="trail">
				{#if isrtl}
					<div class="flex items-center space-x-2">
						<strong class="text-xl uppercase">CO<sub>2</sub>-Footprint</strong>
						<img src="co2.png" alt="CO2 Icon">
						<LightSwitch />
						<button class=" btn btn-sm mr-4" on:click={drawerOpen}>
							<span>
								<svg viewBox="0 0 100 80" class="fill-token w-4 h-4">
									<rect width="100" height="20" />
									<rect y="30" width="100" height="20" />
									<rect y="60" width="100" height="20" />
								</svg>
							</span>
						</button>
					</div>
				{/if}	
				{#if !isrtl}
					<LightSwitch />
					<a
						class="btn btn-sm variant-ghost-surface"
						href="https://github.com/r1tt3rr0st/IPWA01-01_CO2_footprint"
						target="_blank"
						rel="noreferrer"
					>
						GitHub
					</a>
				{/if}

			</svelte:fragment>
		</AppBar>
	</svelte:fragment>
	
	<svelte:fragment slot="pageFooter">
		<div id="pageFooter" class="flex justify-center items-center space-x-2 p-2">
			<div><a class="btn btn-sm variant-ghost-surface" href="/Impressum">Impressum</a></div>
			<div><a class="btn btn-sm variant-ghost-surface" href="/Datenschutz">Datenschutz</a></div>
		</div>
	</svelte:fragment>

	<svelte:fragment slot="footer">
		<div class="grid grid-flow-col justify-center justify-items-center space-x-2">
			<div>
				<small>&copy; Copyright 2023, Muster NGO</small>
			</div>
			<div>
				<small> | </small>
			</div>
			<div>
				<small>
					<a href="https://www.flaticon.com/de/kostenlose-icons/co2-wolke" title="co2-wolke Icons">Co2-wolke Icons erstellt von Freepik - Flaticon</a>
				</small>
			</div>				
		</div>
	</svelte:fragment>

	<slot />
</AppShell>
