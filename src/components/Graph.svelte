<script>
    export let todo_count = [];
	import * as d3 from 'd3';
	import { scaleLinear } from 'd3-scale';
	
	let todo_log_chart;

	// scaling functions
	var bar_width = d3.scaleLinear()
	    .range([0, 500]);

	var bar_color = d3.scaleLinear()
		.range(["#33b578", "#33b578", "#ff7559"])
		.domain([0,4,10]);

	$: {
		// since the cap for the bar length is dynamic, we do it here
		bar_width.domain([0, Math.max(...todo_count)]);

		// define the bars
		let bars = d3.select(todo_log_chart)
			.selectAll("div")
			.data(todo_count.slice().reverse())

		// define what to do when adding a bar
		bars.enter()
			.append("div")
			.style("width", function(d) {
				return bar_width(d) + "px";
			})
			.style("background-color", function(d) {
				return bar_color(d);
			})
			.text(function(d) {
				return d;
			});

		// define what to do with all the bars
		bars.style("width", function(d) {
				return bar_width(d) + "px";
			})
			.style("background-color", function(d) {
				return bar_color(d);
			})
			.text(function(d) {
					return d;
			})
	};
</script>

<div bind:this={todo_log_chart} class="chart"></div>

<style>
	.chart :global(div) {
	font: 25px sans-serif;
	background-color: steelblue;
	padding: 3px;
	border-radius: 5px;
	margin: auto;
	margin-top: 1px;
	text-align: middle;
	color: white;
	}
</style>