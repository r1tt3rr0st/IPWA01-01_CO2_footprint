<script lang="ts">
	import { Accordion, AccordionItem, ListBox, ListBoxItem, popup, TabGroup, Tab, storePopup } from '@skeletonlabs/skeleton';
	import type { PopupSettings } from '@skeletonlabs/skeleton';
	import { onMount } from "svelte";
	import { computePosition, autoUpdate, offset, shift, flip, arrow } from '@floating-ui/dom';

	let isrtl = false;

    onMount(() => {
        const language = navigator.language;
        const countryCode = language.split("-")[0];
        isrtl = ["ar", "he", "en"].includes(countryCode);
    });
	

	let array = [
		{Land: "Deutschland", 	Unternehmen: "Deutsche Telekom", 	col2021: 52990, col2022: 53452, col2023: 62110	},
		{Land: "Deutschland", 	Unternehmen: "SAP", 				col2021: 4275,	col2022: 4156,	col2023: 4159	},
		{Land: "Schweiz", 		Unternehmen: "Novartis", 			col2021: 54385, col2022: 60385, col2023: 64385	},
		{Land: "Schweiz", 		Unternehmen: "AAB", 				col2021: 45372, col2022: 52345, col2023: 49332	},
		{Land: "USA", 			Unternehmen: "Amazon", 				col2021: 76738, col2022: 80738, col2023: 81231	},
		{Land: "USA", 			Unternehmen: "Google", 				col2021: 36353, col2022: 40353, col2023: 43534	},
		{Land: "China", 		Unternehmen: "Alibaba",				col2021: 35278, col2022: 40292, col2023: 43283	},
	];
	
	// Holds table sort state.  Initialized to reflect table sorted by id column ascending.
	let sortBy = {col: "Land", ascending: true};
	
	$: sort = (column: string) => {
		
		if (sortBy.col == column) {
			sortBy.ascending = !sortBy.ascending
		} else {
			sortBy.col = column
			sortBy.ascending = true
		}
		
		// Modifier to sorting function for ascending or descending
		let sortModifier = (sortBy.ascending) ? 1 : -1;
		
		let sort = (a: { [x: string]: string | number; }, b: { [x: string]: string | number; }) => 
			(a[column] < b[column]) 
			? -1 * sortModifier 
			: (a[column] > b[column]) 
			? 1 * sortModifier
			: 0;
		
		filteredArray = filteredArray.sort(sort);
	}

	let resetLaender: string[] = [];
	let unternehmen: string[] = [];
	let laender: string[] = ["China","Deutschland","Schweiz","USA"];

	function handleLaenderToggle() {
		laender = [... resetLaender];
	}

	function laenderHandleOnClick() {
		laender = ["China","Deutschland","Schweiz","USA"];
		unternehmen = [];
	}

	function unternehmenHandleOnClick() {
		unternehmen = ["AAB","Alibaba","Amazon","Deutsche Telekom","Google","Novartis","SAP"];
		handleLaenderToggle();
	}

	$: filteredArray = array.filter(item => laender.includes(item.Land) || unternehmen.includes(item.Unternehmen));

	let tabSet: number = 2;

	storePopup.set({ computePosition, autoUpdate, offset, shift, flip, arrow });

	const popupFeatured: PopupSettings = {
	// Represents the type of event that opens/closed the popup
	event: 'click',
	// Matches the data-popup value on your popup element
	target: 'popupFeatured',
	// Defines which side of your trigger the popup will appear
	placement: 'bottom',
	};

</script>

<div class="container h-full mx-auto flex justify-center items-center">
	<div class="space-y-10 text-center flex flex-col items-center">	
		<h2 class="h2">CO<sub>2</sub>-Emmisionen</h2>
		<p>Hier sind die jährlichen CO<sub>2</sub>-Emissionen von ausgewählten Ländern und Unternehmen aufgelistet.</p>
		<button class="btn btn-sm variant-ghost " use:popup={popupFeatured}>Hinweis</button>
		<div class="card p-4 w-72 shadow-xl" data-popup="popupFeatured">
			<div>Ein erneuter Klick auf den Filter setzt die Auswahl auf "alle Länder anzeigen" bzw. "alle Unternehmen anzeigen" zurück!</div>
			<div class="arrow bg-surface-100-800-token" />
		</div>
			
		

		<TabGroup justify="justify-center">
			{#if !isrtl}
				<Tab bind:group={tabSet} name="2021" value={0}>
					2021
				</Tab>
				<Tab bind:group={tabSet} name="2022" value={1}>
					2022
				</Tab>
				<Tab bind:group={tabSet} name="2023" value={2}>
					2023
				</Tab>
			{/if}

			{#if isrtl}
				<Tab bind:group={tabSet} name="2021" value={2}>
					2023
				</Tab>
				<Tab bind:group={tabSet} name="2022" value={1}>
					2022
				</Tab>
				<Tab bind:group={tabSet} name="2023" value={0}>
					2021
				</Tab>
			{/if}
			<!-- Tab Panels --->
			<svelte:fragment slot="panel">
				{#if tabSet === 0}
					<Accordion width=auto autocollapse>
						<AccordionItem on:click={laenderHandleOnClick} open>
							<svelte:fragment slot="lead">
								<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-funnel" viewBox="0 0 16 16">
								<path d="M1.5 1.5A.5.5 0 0 1 2 1h12a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-.128.334L10 8.692V13.5a.5.5 0 0 1-.342.474l-3 1A.5.5 0 0 1 6 14.5V8.692L1.628 3.834A.5.5 0 0 1 1.5 3.5v-2zm1 .5v1.308l4.372 4.858A.5.5 0 0 1 7 8.5v5.306l2-.666V8.5a.5.5 0 0 1 .128-.334L13.5 3.308V2h-11z"/>
								</svg>
							</svelte:fragment>
							<svelte:fragment slot="summary">Nach Land filtern</svelte:fragment>
							<svelte:fragment slot="content">
								<ListBox multiple>
									<ListBoxItem bind:group={laender} name="laender" value="China">China</ListBoxItem>
									<ListBoxItem bind:group={laender} name="laender" value="Deutschland">Deutschland</ListBoxItem>
									<ListBoxItem bind:group={laender} name="laender" value="Schweiz">Schweiz</ListBoxItem>
									<ListBoxItem bind:group={laender} name="laender" value="USA">USA</ListBoxItem>
								</ListBox>
							</svelte:fragment>
						</AccordionItem>
						<AccordionItem on:click={unternehmenHandleOnClick}>
							<svelte:fragment slot="lead">
								<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-funnel" viewBox="0 0 16 16">
								<path d="M1.5 1.5A.5.5 0 0 1 2 1h12a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-.128.334L10 8.692V13.5a.5.5 0 0 1-.342.474l-3 1A.5.5 0 0 1 6 14.5V8.692L1.628 3.834A.5.5 0 0 1 1.5 3.5v-2zm1 .5v1.308l4.372 4.858A.5.5 0 0 1 7 8.5v5.306l2-.666V8.5a.5.5 0 0 1 .128-.334L13.5 3.308V2h-11z"/>
								</svg>
							</svelte:fragment>
							<svelte:fragment slot="summary">Nach Unternehmen filtern</svelte:fragment>
							<svelte:fragment slot="content">
								<ListBox multiple>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="AAB">AAB</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Alibaba">Alibaba</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Amazon">Amazon</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Deutsche Telekom">Deutsche Telekom</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Google">Google</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Novartis">Novartis</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="SAP">SAP</ListBoxItem>
								</ListBox>
							</svelte:fragment>
						</AccordionItem>
					</Accordion>
			
					<div class="relative overflow-x-auto shadow-md rounded-lg">
						<table class="w-full text-md text-left text-gray-500 dark:text-gray-400">
							<thead class="text-xs text-gray-50 uppercase bg-gray-500 dark:bg-gray-700 dark:text-gray-50">
								<tr>
									<th on:click={() => sort("Land")} scope="col" class="px-3 py-2">
										<div class="flex items-center">
											Land
											<svg class="w-3 h-3 ml-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
												<path d="M8.574 11.024h6.852a2.075 2.075 0 0 0 1.847-1.086 1.9 1.9 0 0 0-.11-1.986L13.736 2.9a2.122 2.122 0 0 0-3.472 0L6.837 7.952a1.9 1.9 0 0 0-.11 1.986 2.074 2.074 0 0 0 1.847 1.086Zm6.852 1.952H8.574a2.072 2.072 0 0 0-1.847 1.087 1.9 1.9 0 0 0 .11 1.985l3.426 5.05a2.123 2.123 0 0 0 3.472 0l3.427-5.05a1.9 1.9 0 0 0 .11-1.985 2.074 2.074 0 0 0-1.846-1.087Z"/>
											</svg>
										</div>
									</th>
									<th on:click={() => sort("Unternehmen")} scope="col" class="px-3 py-2">
										<div class="flex items-center">
											Unternehmen
											<svg class="w-3 h-3 ml-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
												<path d="M8.574 11.024h6.852a2.075 2.075 0 0 0 1.847-1.086 1.9 1.9 0 0 0-.11-1.986L13.736 2.9a2.122 2.122 0 0 0-3.472 0L6.837 7.952a1.9 1.9 0 0 0-.11 1.986 2.074 2.074 0 0 0 1.847 1.086Zm6.852 1.952H8.574a2.072 2.072 0 0 0-1.847 1.087 1.9 1.9 0 0 0 .11 1.985l3.426 5.05a2.123 2.123 0 0 0 3.472 0l3.427-5.05a1.9 1.9 0 0 0 .11-1.985 2.074 2.074 0 0 0-1.846-1.087Z"/>
											</svg>
										</div>
									</th>
									<th on:click={() => sort("col2021")} scope="col" class="px-3 py-2">
										<div class="flex items-center">
											CO<sub>2</sub>-Emmisionen (Tonnen)
											<svg class="w-3 h-3 ml-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
												<path d="M8.574 11.024h6.852a2.075 2.075 0 0 0 1.847-1.086 1.9 1.9 0 0 0-.11-1.986L13.736 2.9a2.122 2.122 0 0 0-3.472 0L6.837 7.952a1.9 1.9 0 0 0-.11 1.986 2.074 2.074 0 0 0 1.847 1.086Zm6.852 1.952H8.574a2.072 2.072 0 0 0-1.847 1.087 1.9 1.9 0 0 0 .11 1.985l3.426 5.05a2.123 2.123 0 0 0 3.472 0l3.427-5.05a1.9 1.9 0 0 0 .11-1.985 2.074 2.074 0 0 0-1.846-1.087Z"/>
											</svg>
										</div>
									</th>
								</tr>
							</thead>
							<tbody>
								{#each filteredArray as row}
									<tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
										<td class="px-3 py-2 font-medium text-gray-900 whitespace-normal dark:text-white">{row.Land}</td>
										<td class="px-3 py-2 font-medium text-gray-900 whitespace-normal dark:text-white">{row.Unternehmen}</td>
										<td class="px-3 py-2 font-medium text-gray-900 whitespace-normal dark:text-white">{row.col2021}</td>
									</tr>
								{/each}
							</tbody>
						</table>
					</div>

				{:else if tabSet === 1}
					<Accordion width=auto autocollapse>
						<AccordionItem on:click={laenderHandleOnClick} open>
							<svelte:fragment slot="lead">
								<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-funnel" viewBox="0 0 16 16">
								<path d="M1.5 1.5A.5.5 0 0 1 2 1h12a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-.128.334L10 8.692V13.5a.5.5 0 0 1-.342.474l-3 1A.5.5 0 0 1 6 14.5V8.692L1.628 3.834A.5.5 0 0 1 1.5 3.5v-2zm1 .5v1.308l4.372 4.858A.5.5 0 0 1 7 8.5v5.306l2-.666V8.5a.5.5 0 0 1 .128-.334L13.5 3.308V2h-11z"/>
								</svg>
							</svelte:fragment>
							<svelte:fragment slot="summary">Nach Land filtern</svelte:fragment>
							<svelte:fragment slot="content">
								<ListBox multiple>
									<ListBoxItem bind:group={laender} name="laender" value="China">China</ListBoxItem>
									<ListBoxItem bind:group={laender} name="laender" value="Deutschland">Deutschland</ListBoxItem>
									<ListBoxItem bind:group={laender} name="laender" value="Schweiz">Schweiz</ListBoxItem>
									<ListBoxItem bind:group={laender} name="laender" value="USA">USA</ListBoxItem>
								</ListBox>
							</svelte:fragment>
						</AccordionItem>
						<AccordionItem on:click={unternehmenHandleOnClick}>
							<svelte:fragment slot="lead">
								<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-funnel" viewBox="0 0 16 16">
								<path d="M1.5 1.5A.5.5 0 0 1 2 1h12a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-.128.334L10 8.692V13.5a.5.5 0 0 1-.342.474l-3 1A.5.5 0 0 1 6 14.5V8.692L1.628 3.834A.5.5 0 0 1 1.5 3.5v-2zm1 .5v1.308l4.372 4.858A.5.5 0 0 1 7 8.5v5.306l2-.666V8.5a.5.5 0 0 1 .128-.334L13.5 3.308V2h-11z"/>
								</svg>
							</svelte:fragment>
							<svelte:fragment slot="summary">Nach Unternehmen filtern</svelte:fragment>
							<svelte:fragment slot="content">
								<ListBox multiple>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="AAB">AAB</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Alibaba">Alibaba</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Amazon">Amazon</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Deutsche Telekom">Deutsche Telekom</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Google">Google</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Novartis">Novartis</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="SAP">SAP</ListBoxItem>
								</ListBox>
							</svelte:fragment>
						</AccordionItem>
					</Accordion>
			
					<div class="relative overflow-x-auto shadow-md rounded-lg">
						<table class="w-full text-md text-left text-gray-500 dark:text-gray-400">
							<thead class="text-xs text-gray-50 uppercase bg-gray-500 dark:bg-gray-700 dark:text-gray-50">
								<tr>
									<th on:click={() => sort("Land")} scope="col" class="px-3 py-2">
										<div class="flex items-center">
											Land
											<svg class="w-3 h-3 ml-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
												<path d="M8.574 11.024h6.852a2.075 2.075 0 0 0 1.847-1.086 1.9 1.9 0 0 0-.11-1.986L13.736 2.9a2.122 2.122 0 0 0-3.472 0L6.837 7.952a1.9 1.9 0 0 0-.11 1.986 2.074 2.074 0 0 0 1.847 1.086Zm6.852 1.952H8.574a2.072 2.072 0 0 0-1.847 1.087 1.9 1.9 0 0 0 .11 1.985l3.426 5.05a2.123 2.123 0 0 0 3.472 0l3.427-5.05a1.9 1.9 0 0 0 .11-1.985 2.074 2.074 0 0 0-1.846-1.087Z"/>
											</svg>
										</div>
									</th>
									<th on:click={() => sort("Unternehmen")} scope="col" class="px-3 py-2">
										<div class="flex items-center">
											Unternehmen
											<svg class="w-3 h-3 ml-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
												<path d="M8.574 11.024h6.852a2.075 2.075 0 0 0 1.847-1.086 1.9 1.9 0 0 0-.11-1.986L13.736 2.9a2.122 2.122 0 0 0-3.472 0L6.837 7.952a1.9 1.9 0 0 0-.11 1.986 2.074 2.074 0 0 0 1.847 1.086Zm6.852 1.952H8.574a2.072 2.072 0 0 0-1.847 1.087 1.9 1.9 0 0 0 .11 1.985l3.426 5.05a2.123 2.123 0 0 0 3.472 0l3.427-5.05a1.9 1.9 0 0 0 .11-1.985 2.074 2.074 0 0 0-1.846-1.087Z"/>
											</svg>
										</div>
									</th>
									<th on:click={() => sort("col2022")} scope="col" class="px-3 py-2">
										<div class="flex items-center">
											CO<sub>2</sub>-Emmisionen (Tonnen)
											<svg class="w-3 h-3 ml-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
												<path d="M8.574 11.024h6.852a2.075 2.075 0 0 0 1.847-1.086 1.9 1.9 0 0 0-.11-1.986L13.736 2.9a2.122 2.122 0 0 0-3.472 0L6.837 7.952a1.9 1.9 0 0 0-.11 1.986 2.074 2.074 0 0 0 1.847 1.086Zm6.852 1.952H8.574a2.072 2.072 0 0 0-1.847 1.087 1.9 1.9 0 0 0 .11 1.985l3.426 5.05a2.123 2.123 0 0 0 3.472 0l3.427-5.05a1.9 1.9 0 0 0 .11-1.985 2.074 2.074 0 0 0-1.846-1.087Z"/>
											</svg>
										</div>
									</th>
								</tr>
							</thead>
							<tbody>
								{#each filteredArray as row}
									<tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
										<td class="px-3 py-2 font-medium text-gray-900 whitespace-normal dark:text-white">{row.Land}</td>
										<td class="px-3 py-2 font-medium text-gray-900 whitespace-normal dark:text-white">{row.Unternehmen}</td>
										<td class="px-3 py-2 font-medium text-gray-900 whitespace-normal dark:text-white">{row.col2022}</td>
									</tr>
								{/each}
							</tbody>
						</table>
					</div>
				{:else if tabSet === 2}
						<Accordion width=auto autocollapse>
						<AccordionItem on:click={laenderHandleOnClick} open>
							<svelte:fragment slot="lead">
								<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-funnel" viewBox="0 0 16 16">
								<path d="M1.5 1.5A.5.5 0 0 1 2 1h12a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-.128.334L10 8.692V13.5a.5.5 0 0 1-.342.474l-3 1A.5.5 0 0 1 6 14.5V8.692L1.628 3.834A.5.5 0 0 1 1.5 3.5v-2zm1 .5v1.308l4.372 4.858A.5.5 0 0 1 7 8.5v5.306l2-.666V8.5a.5.5 0 0 1 .128-.334L13.5 3.308V2h-11z"/>
								</svg>
							</svelte:fragment>
							<svelte:fragment slot="summary">Nach Land filtern</svelte:fragment>
							<svelte:fragment slot="content">
								<ListBox multiple>
									<ListBoxItem bind:group={laender} name="laender" value="China">China</ListBoxItem>
									<ListBoxItem bind:group={laender} name="laender" value="Deutschland">Deutschland</ListBoxItem>
									<ListBoxItem bind:group={laender} name="laender" value="Schweiz">Schweiz</ListBoxItem>
									<ListBoxItem bind:group={laender} name="laender" value="USA">USA</ListBoxItem>
								</ListBox>
							</svelte:fragment>
						</AccordionItem>
						<AccordionItem on:click={unternehmenHandleOnClick}>
							<svelte:fragment slot="lead">
								<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-funnel" viewBox="0 0 16 16">
								<path d="M1.5 1.5A.5.5 0 0 1 2 1h12a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-.128.334L10 8.692V13.5a.5.5 0 0 1-.342.474l-3 1A.5.5 0 0 1 6 14.5V8.692L1.628 3.834A.5.5 0 0 1 1.5 3.5v-2zm1 .5v1.308l4.372 4.858A.5.5 0 0 1 7 8.5v5.306l2-.666V8.5a.5.5 0 0 1 .128-.334L13.5 3.308V2h-11z"/>
								</svg>
							</svelte:fragment>
							<svelte:fragment slot="summary">Nach Unternehmen filtern</svelte:fragment>
							<svelte:fragment slot="content">
								<ListBox multiple>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="AAB">AAB</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Alibaba">Alibaba</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Amazon">Amazon</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Deutsche Telekom">Deutsche Telekom</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Google">Google</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="Novartis">Novartis</ListBoxItem>
									<ListBoxItem bind:group={unternehmen} name="unternehmen" value="SAP">SAP</ListBoxItem>
								</ListBox>
							</svelte:fragment>
						</AccordionItem>
					</Accordion>
			
					<div class="relative overflow-x-auto shadow-md rounded-lg">
						<table class="w-full text-md text-left text-gray-500 dark:text-gray-400">
							<thead class="text-xs text-gray-50 uppercase bg-gray-500 dark:bg-gray-700 dark:text-gray-50">
								<tr>
									<th on:click={() => sort("Land")} scope="col" class="px-3 py-2">
										<div class="flex items-center">
											Land
											<svg class="w-3 h-3 ml-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
												<path d="M8.574 11.024h6.852a2.075 2.075 0 0 0 1.847-1.086 1.9 1.9 0 0 0-.11-1.986L13.736 2.9a2.122 2.122 0 0 0-3.472 0L6.837 7.952a1.9 1.9 0 0 0-.11 1.986 2.074 2.074 0 0 0 1.847 1.086Zm6.852 1.952H8.574a2.072 2.072 0 0 0-1.847 1.087 1.9 1.9 0 0 0 .11 1.985l3.426 5.05a2.123 2.123 0 0 0 3.472 0l3.427-5.05a1.9 1.9 0 0 0 .11-1.985 2.074 2.074 0 0 0-1.846-1.087Z"/>
											</svg>
										</div>
									</th>
									<th on:click={() => sort("Unternehmen")} scope="col" class="px-3 py-2">
										<div class="flex items-center">
											Unternehmen
											<svg class="w-3 h-3 ml-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
												<path d="M8.574 11.024h6.852a2.075 2.075 0 0 0 1.847-1.086 1.9 1.9 0 0 0-.11-1.986L13.736 2.9a2.122 2.122 0 0 0-3.472 0L6.837 7.952a1.9 1.9 0 0 0-.11 1.986 2.074 2.074 0 0 0 1.847 1.086Zm6.852 1.952H8.574a2.072 2.072 0 0 0-1.847 1.087 1.9 1.9 0 0 0 .11 1.985l3.426 5.05a2.123 2.123 0 0 0 3.472 0l3.427-5.05a1.9 1.9 0 0 0 .11-1.985 2.074 2.074 0 0 0-1.846-1.087Z"/>
											</svg>
										</div>
									</th>
									<th on:click={() => sort("col2023")} scope="col" class="px-3 py-2">
										<div class="flex items-center">
											CO<sub>2</sub>-Emmisionen (Tonnen)
											<svg class="w-3 h-3 ml-1.5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
												<path d="M8.574 11.024h6.852a2.075 2.075 0 0 0 1.847-1.086 1.9 1.9 0 0 0-.11-1.986L13.736 2.9a2.122 2.122 0 0 0-3.472 0L6.837 7.952a1.9 1.9 0 0 0-.11 1.986 2.074 2.074 0 0 0 1.847 1.086Zm6.852 1.952H8.574a2.072 2.072 0 0 0-1.847 1.087 1.9 1.9 0 0 0 .11 1.985l3.426 5.05a2.123 2.123 0 0 0 3.472 0l3.427-5.05a1.9 1.9 0 0 0 .11-1.985 2.074 2.074 0 0 0-1.846-1.087Z"/>
											</svg>
										</div>
									</th>
								</tr>
							</thead>
							<tbody>
								{#each filteredArray as row}
									<tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
										<td class="px-3 py-2 font-medium text-gray-900 whitespace-normal dark:text-white">{row.Land}</td>
										<td class="px-3 py-2 font-medium text-gray-900 whitespace-normal dark:text-white">{row.Unternehmen}</td>
										<td class="px-3 py-2 font-medium text-gray-900 whitespace-normal dark:text-white">{row.col2023}</td>
									</tr>
								{/each}
							</tbody>
						</table>
					</div>
				{/if}
			</svelte:fragment>
		</TabGroup>
	</div>
</div>
