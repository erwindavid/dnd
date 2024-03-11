<script>
    import Table from './Table.svelte';
    import Row from './TableRow.svelte';
	import Relation from './Relation.svelte';
	import raw from './data.json';

	let data = raw.data
    let positions = [{ x: 40, y: 100 },{ x: 240, y: 220 },{ x:270, y:50 }];
    $ : filteredata = data;
    $ : filtered = false;
	$ : tables = filteredata.reduce((acc, cur)=>{
        let table = Object.values(cur)[0]
        if(!acc.some(e=>e.table == table)) {
            acc.push({table: table, rows : 0, pos: positions[Object.keys(acc).length]})
        }
        acc.map(e => {
          if (e.table == table) {
            e.rows ++
          }
          return e
        })
        return acc
    },[]);
    
    $ : relations = data.filter(e=> e.fk !== '').map(e=> {return {... tables.filter(f=>f.table==e.table).map(f=>{return {x1:f.pos.x, y1:f.pos.y}})[0], ...tables.filter(f=>f.table==e.fk.split('.')[0]).map(f=>{return {x2:f.pos.x, y2:f.pos.y}})[0]}});
    
    function toggle(){
        filtered = !filtered
        filteredata = filtered ? data.filter((d)=> d.dtype === 'T') : data
    };
    
    function search(e){
    	const formData = new FormData(e.target)
	    for (let field of formData) {
	      filteredata = data.filter((d)=> d.row.includes(field[1]))
    	}
    }
    
</script>

<div class="header">
	<form on:submit|preventDefault={search}>
		<label>Search</label>
		<input name="search" id="search" type="text" value=""/>
		<button type="submit">Submit</button>
	</form>
</div>
<div class="viz">
	{#each relations as r}
		<Relation x1={r.x1} y1={r.y1} x2={r.x2} y2={r.y2}/>
	{/each}  
	{#each tables as t}
		<Table name={t.table} bind:position={t.pos} bind:rows={t.rows}>
	    	{#each filteredata as r, i}
	        	{#if r.table == t.table}
	            	<Row row={r.row} num={i}/>
	        	{/if}
	    	{/each}
		</Table>
	{/each}
</div>
<table>
	<tr>
		<th>ty</th>
	</tr>
</table>

<style>
	.header{
		background-color: darkslateblue;
	}
	
	.viz{
		background-color: yellow;
	}
</style>