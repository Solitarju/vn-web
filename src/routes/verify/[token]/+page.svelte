<script lang="ts">
    import { page } from '$app/state';
    import { onMount } from 'svelte';
    import InternalServerError from '../../Components/InternalServerError.svelte';
    import NotFound from '../../Components/NotFound.svelte';
    import Pending from '../../Components/Pending.svelte';
    import Verified from '../../Components/Verified.svelte';

    let promise: Promise<Response> = $state(new Promise(() => {}));

    // fetch promise is set in onMount so that the browser doesn't prefetch to see if fetch is permitted with CORS, dont change this.
    onMount(() => {
        promise = fetch(`https://vn.solitarju.uk/verify/${page.params.token}`, { method: 'DELETE' });
    })
</script>

{#await promise }
	<Pending />
{:then res} 
	{#if res?.status == 204}
        <Verified />
    {:else if res?.status == 404}
        <NotFound />
    {:else}
        <InternalServerError />
    {/if}
{:catch}
    <InternalServerError />
{/await}