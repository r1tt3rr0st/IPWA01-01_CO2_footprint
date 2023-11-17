<script lang="ts">
    import { getDrawerStore } from "@skeletonlabs/skeleton";
    import { onMount } from "svelte";

    let isrtl = false;

    onMount(() => {
        const language = navigator.language;
        const countryCode = language.split("-")[0];
        isrtl = ["ar", "he"].includes(countryCode);
    });

    const drawerStore = getDrawerStore();
    
    function drawerClose(): void {
        drawerStore.close();
    }

</script>

<nav class="list-nav p-4">
    {#if !isrtl}
        <ul>
            <li><a href="/" on:click={drawerClose}>Startseite</a></li>
            <li><a href="/Emissionen" on:click={drawerClose}>CO&#8322;-Emissionen</a></li>
            <li><a href="/Kontakt" on:click={drawerClose}>Kontakt</a></li>
            <li><a href="/Impressum" on:click={drawerClose}>Impressum</a></li>
            <li><a href="/Datenschutz" on:click={drawerClose}>Datenschutz</a></li>	
        </ul>
    {/if}
    {#if isrtl}
        <ul dir="rtl">
            <li><a href="/" on:click={drawerClose}>Startseite</a></li>
            <li><a href="/Emissionen" on:click={drawerClose}>CO&#8322;-Emissionen</a></li>
            <li><a href="/Kontakt" on:click={drawerClose}>Kontakt</a></li>
            <li><a href="/Impressum" on:click={drawerClose}>Impressum</a></li>
            <li><a href="/Datenschutz" on:click={drawerClose}>Datenschutz</a></li>	
        </ul>
    {/if}
</nav>


				