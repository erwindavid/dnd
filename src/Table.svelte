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

	function drag2(e){
    position.x = e.clientX - touched.x;
    position.y = e.clientY - touched.y;
	}

    function resize(e) {
    height = e.touches[0].clientY - position.y - 50;
    }

	function resize2(e) {
    height = e.clientY - position.y - 50;
    }

	function tgm(e) {
	touched.x = e.clientX - e.target.parentElement.offsetLeft;
	touched.y = e.clientY - e.target.parentElement.offsetTop;
	document.addEventListener('mousemove',resize2)
	document.addEventListener('mouseup',()=> {
		document.removeEventListener('mousemove',resize2)
		})
	}

	function touch2(e) {
	touched.x = e.clientX - e.target.parentElement.offsetLeft;
	touched.y = e.clientY - e.target.parentElement.offsetTop;
	document.addEventListener('mousemove',drag2)
	document.addEventListener('mouseup',()=> {
		document.removeEventListener('mousemove',drag2)
		})
	}


</script>

<div 
	class="container" 
	style={`top:${position.y}px; left:${position.x}px; height: ${maxheight}px;`}
>
    <div 
		class="header" 
		on:touchstart={touch} 
		on:touchmove={drag} 
		on:mousedown={touch2}
	>{name}</div>
    <div class="body">
        <slot/>
    </div>
    <div 
		class="resizer"
		on:touchmove={resize} 
		on:mousedown={tgm}
	></div>
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
	.header:hover {
	cursor: grab;
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
