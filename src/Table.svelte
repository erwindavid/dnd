<script>
    export let name;
    export let position;
    export let rows;
    
    $ : height = rows * 25;
    $ : maxheight = height>150?150:height

    let touched = { x: 0, y: 0};

    function touch(e) {
    touched.x = e.touches[0].clientX - e.target.parentElement.offsetLeft;
    touched.y = e.touches[0].clientY - e.target.parentElement.offsetTop;
    }
    function drag(e) {
    position.x = e.touches[0].clientX - touched.x;
    position.y = e.touches[0].clientY - touched.y;
    }
    function resize(e) {
    height = e.touches[0].clientY - position.y - 50;
    }
</script>

<div class="container" style={`top:${position.y}px; left:${position.x}px; height: ${maxheight}px;`}>
    <div class="header" on:touchstart={touch} on:touchmove={drag}>{name}</div>
    <div class="body">
        <slot/>
    </div>
    <div class="resizer" on:touchmove={resize}></div>
</div>

<style>

    .container {
    position: absolute;
    width: 150px;
    color: white;
    }
    .header {
    height: 30px;
    background-color: teal;
    }
    .body {
    overflow-y: scroll;
    height: 100%;
    background-color: brown;
    }
    .resizer {
    height: 30px;
    background-color: grey;
    }
</style>