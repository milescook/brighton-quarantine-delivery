<script>
    import { createEventDispatcher } from 'svelte';

    export let filters;
    export let filtered;

    const dispatch = createEventDispatcher();

    let selected = filtered ? filtered : 'all';

    function updateFilter(event) {
        if (selected) {
            dispatch('updatefilter', selected);
        }
    }

    if (typeof window !== 'undefined') {
        // If the user uses the forward or backward button - set the select box and content
        window.onpopstate = function(event) {
            const urlParams = new URLSearchParams(window.location.search);
            
            // Get the parameter from the URL and update selected
            selected = urlParams.has('filter') ? urlParams.get('filter') : 'all';
            // Update the content
            dispatch('updatefilter', selected);
        }
    }
</script>

<style>
    .filters {
        grid-column: span 12;
    }

    .filters__label {
        color: var(--blue);
        font-family: var(--text-font);
        font-size: 1.4rem;
    }

    .filters__select {
        background-color: var(--grey);
        border: 1px solid var(--gold);
        font-family: var(--text-font);
        font-size: 1.4rem;
    }

    .filters__select:active,
    .filters__select:focus,
    .filters__select:hover {
        outline: none;
    }
</style>

<div class="filters">
    <label class="filters__label" for="categories">Filter by category:</label>

    <select bind:value={selected} class="filters__select" id="categories" name="categories" on:change={updateFilter}>
        <option value="all">All</option>

        {#each filters as item}
            {#if item}
                <option value={item}>{item}</option>
            {/if}
        {/each}
    </select>
</div>