<script>
    export let todo_count = [];
	import * as d3 from 'd3';
	import { scaleLinear } from 'd3-scale';
	
	let todo_line_chart;

</script>

<div bind:this={todo_line_chart} class="chart"></div>

<style>
	.chart :global(div) {
	font: 25px sans-serif;
	background-color: steelblue;
	padding: 3px;
	border-radius: 5px;
	margin: auto;
	margin-top: 1px;
	text-align: middle;
	color: #222;
	}
</style>