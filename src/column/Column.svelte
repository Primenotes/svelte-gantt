<script lang="ts">
    export let left;
    export let width;
</script>

<div class="column" style="left:{left}px;width:{width}px">y</div>
<style>
    .column {
        position: absolute;
        height: 100%;
        border-left: 1px solid #a00707;
    }
    
</style>