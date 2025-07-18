<script lang="js">
    //@ts-nocheck

    import { onMount } from 'svelte';
	import { Section } from 'flowbite-svelte-blocks';
	import { Button, Drawer, CloseButton, Heading, Alert, Modal } from 'flowbite-svelte';
	import Map from './Map.svelte';
    import { getImage } from '$lib/model';
    import { getCachedImgs } from '$lib/cache';
	import { writable } from 'svelte/store';
    import AddLocForm from './AddLocForm.svelte';

    const SEND_URL = import.meta.env.VITE_URL;

    let showImgModal = writable(false);
    let showAddLoc = writable(false);

    let images = [];

    let categories = [];

    onMount(async () => {

        console.log(mapData);

        let imgs = getCachedImgs();
        console.log(imgs);

        if (imgs) images = imgs[mapData.id];
        else images = await getImage(mapData.id)

    });

    export let mapData = {"title": "Loading ...", "id": "rehaan"}
</script>

<style>
    .contentwrapper {
        display: flex;
        justify-content: space-around;
    }

    .textwrapper {
        min-width: 20rem;
        max-width: 35rem;
    }

    .valuewrapper {
        margin: 1rem;
        font-size: large;
    }

    .imgswrapper {
        display: flex;
        justify-content: space-around;
        flex-wrap: wrap;
    }

    .indimgwrapper {
        min-width: 40%;
        min-height: 10%;
        max-width: 50%;
        max-height: 50%;
        padding: 1rem;
    }
</style>

<Modal class="min-w-full" open={$showAddLoc} on:close={() => {showAddLoc.set(false); }} size="xl">
    <AddLocForm addEdit={"Edit"} editVal={mapData}></AddLocForm>
</Modal>

<p class="text-3xl text-gray-900" style="margin-bottom: 1rem;">
    <Button pill color="blue" on:click={() => {showAddLoc.set(true);}}>Edit</Button>
    {mapData.title}
</p>

<div class="contentwrapper">

    <div class="textwrapper">
        {#each Object.entries(mapData) as [key, value]}
            {#if key != "id" && key != "category" && key != "title" && key != "coords" && key != "addCoor" && key != "newLoc" && key != "moreData" && key != "docId"}

                <Alert color="dark">
                    <span class="font-medium">{key}</span>
                    <div class="valuewrapper">{value}</div>
                </Alert>

            {/if}
        {/each}
    </div>

    {#if images}

        <div class="imgswrapper">
            {#each images as i}

                <div class="indimgwrapper">
                    <img style="border-radius:5px" src={i} alt="david-fichter-mural">
                </div>

            {/each}
        </div>

    {/if}

</div>
