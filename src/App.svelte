<script>
    import Table from './Table.svelte';
    import Row from './TableRow.svelte';
	import Relation from './Relation.svelte';
	import raw from './data.json';

	let data = raw.data

	let editingRow
	let editingColumnName
	
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
	function addRow(){
		console.log('addRow')
		filteredata.push(Array.from(document.querySelectorAll('#rw')).reduce((acc, cur, id)=>{
			acc[Object.keys(data[0])[id]] = cur.value
			return acc
		},{}))
		positions = positions
		filteredata = filteredata
	}

	function toggleEditing(e) {
		editingRow = editingRow == e ? null: e

	}

	function deleteRow(e){
		data.removeIndex(e)
		editingRow =null
	}

	function updateViz(){
		console.log(positions[0]['x'])
		positions[0]['x'] = positions[0]['x']+2
		console.log(positions[0]['x'])
	}

	function handleChange(e){
		data.filter(e=>e.row == editingRow.row && e.table == editingRow.table)[0][editingColumnName] = e.target.value
		updateViz()
	}

	function sendNumber(e){
		editingColumnName = Object.keys(data[0])[e] 
	}

	Array.prototype.removeIndex = function(id){
		return this.splice(id,1)
	}

</script>

<div class="cont">

<div class="header">
	<form on:submit|preventDefault={search}>
		<label>Search</label>
		<input name="search" id="search" type="text" value=""/>
		<button type="submit">Submit</button>
	</form>
</div>

<div class="data">
	<table>
		<tr>
		<th></th>
		{#each Object.keys(data[0]) as head}
			<th>{head}</th>
		{/each}
		</tr>
		{#each filteredata as row, j}
		<tr>
			<td>
				<button on:click={()=>toggleEditing(row)} type="button">E</button>
				<button type="button" on:click={()=>deleteRow(j)}>D</button>
			</td>
			{#each Object.values(row) as col, i}
			{#if editingRow==row}
				<td><input type="text" value={col} on:input={()=>sendNumber(i)} on:input={handleChange} ></td>
			{:else}
				<td>{col}</td>
			{/if}
			{/each}
		</tr>
		{/each}
		<tr id="newrow">
			<td></td>
			<td><input id="rw" type="text"></td>
			<td><input id="rw" type="text"></td>
			<td><input id="rw" type="text"></td>
			<td><input id="rw" type="text"></td>
			<td><input id="rw" type="text"></td>
		</tr>
	</table>
<button on:click|preventDefault={addRow} type="button">Add row</button>
<div>ph</div>
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
</div>
<style>
	.header{
		grid-area: header;
		background-color: darkslateblue;
	}
	
	.viz{
		grid-area: viz;
		position: relative;
		background-color: darkgrey;
		overflow: scroll;
		height: 100vh
	}

	.data {
		grid-area: data;
		overflow: scroll;
		height: 60vh;
	}

	.cont {
		display: grid;
		grid-template-areas: 
			'header header header header'
			'data viz viz viz'
	}

	table {
		font-weight: normal;
		background-color: darkgrey;
		width: 100%;
		font-size: smaller;
	}

	th, td {
		border: 1px solid lightgray;
	}

	td {
		font-weight: normal;
	}

	th {
		position: sticky;
	}

	input{
		width:20px;
	}
	.red{
		color: red;
	}

</style>
