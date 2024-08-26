<script>
    import {count} from "../stores.js";

    const onIncre = ()=>{
        count.increment();
    }
</script>

<button on:click={onIncre}>+</button>