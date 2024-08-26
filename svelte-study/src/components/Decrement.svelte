<script>
    import {count} from "../stores.js";

    const onDecre = ()=>{
        count.decrement();
    }
</script>

<button on:click={onDecre}>-</button>