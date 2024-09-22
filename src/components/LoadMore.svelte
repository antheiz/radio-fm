<script>
    import { createEventDispatcher } from 'svelte';
    import RadioCard from './RadioCard.svelte';

    export let radioList = [];
    export let initialCount = 6;
    export let loadCount = 3;

    let currentIndex = initialCount;
    const dispatch = createEventDispatcher();

    function loadMore() {
        const remainingItems = radioList.length - currentIndex;
        const itemsToLoad = Math.min(loadCount, remainingItems);
        currentIndex += itemsToLoad;
        dispatch('load', { currentIndex });
    }

    $: visibleRadios = radioList.slice(0, currentIndex);
    $: showLoadMore = currentIndex < radioList.length;
</script>

<div class="flex flex-col gap-3 max-w-3xl px-4 sm:px-6 lg:px-8 mx-auto">
    {#each visibleRadios as radio}
        <RadioCard
            title={radio.title}
            description={radio.description}
            url={radio.url}
        />
    {/each}
</div>

{#if showLoadMore}
    <div class="max-w-3xl mx-auto text-right mt-8 px-4 lg:px-8">
        <button
        on:click={loadMore}
        class="inline-block rounded border border-gray-200 px-8 py-2.5 text-sm font-medium text-gray-600 transition hover:rotate-2 hover:scale-110 focus:outline-none active:text-gray-500"
        >
        Load more
        </button>
    </div>
{/if}
