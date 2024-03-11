<script>
	export let x1;
	export let x2;
	export let y1;
	export let y2;
	
	$ : mx1 = x2 > x1 ? x1+150 : x1;
	$ : mx2 = x2 > x1 ? x2 : x2+150;
	$ : my1 = y1+15;
	$ : my2 = y2+15;
	$ : minx = mx1 > mx2 ? mx2 : mx1;
	$ : miny = my1 > my2 ? my2 : my1;
	$ : width = mx1 > mx2 ? mx1-mx2: mx2-mx1;
	$ : height = my1 > my2 ? my1-my2 : my2-my1;
	$ : viewbox = [minx, miny, width, height]
</script>

<svg class="container" height={height} width={width} style={`top: ${miny}px; left: ${minx}px`} viewbox={viewbox}>
		<path d={`M ${mx1} ${my1} L ${mx2} ${my2}`} stroke="grey" stroke-width="3" stroke-linejoin="round"/>
</svg>

<style>
	.container {
		position: absolute;
	}
</style>