<script>
    export let count;
    export let handleClick;

    import LabelComp from "./LabelComp.svelte"
</script>

<button on:click={handleClick}>
    클릭수 {count} <LabelComp {count} />
</button>