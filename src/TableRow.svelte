<script>
    export let row;
    export let num;
    
    function bckgco(num) {
    let b = num%2 == 0?'even': 'odd'
    return 'row '+b
    }
</script>

<div class={bckgco(num)}>
    {row}
</div>

<style>

    .row {
    height: 25px;
    position: relative;
    }
    .odd {
    background-color: darkred;
    }
    .even {
    bakground-color: red;
    }

</style>