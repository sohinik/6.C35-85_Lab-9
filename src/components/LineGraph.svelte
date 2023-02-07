<script>
    export let todo_count_formatted = [];
    import * as d3 from "d3";
    import { scaleLinear } from "d3-scale";
    import { onMount } from "svelte";

    let my_dataviz;

    // Create 2 datasets
    let todo_count_formatted_last_5 = [];
    $: todo_count_formatted_last_5 = todo_count_formatted.slice(
        Math.max(todo_count_formatted.length - 5, 0)
    );

    // set the dimensions and margins of the graph
    let margin = { top: 10, right: 30, bottom: 30, left: 50 },
        width = 400 - margin.left - margin.right,
        height = 300 - margin.top - margin.bottom;

    // Initialise a X axis:
    var x = d3.scaleLinear().range([0, width]);
    var xAxis = d3.axisBottom().scale(x);

    // Initialize an Y axis
    var y = d3.scaleLinear().range([height, 0]);
    var yAxis = d3.axisLeft().scale(y);

    let charts;

    $: {
        // append the svg object to the body of the page
        charts = d3
            .select(my_dataviz)
            .append("svg")
            .style("width", width + margin.left + margin.right)
            .style("height", height + margin.top + margin.bottom)
            .append("g")
            .attr(
                "transform",
                "translate(" + margin.left + "," + margin.top + ")"
            );

        charts
            .append("g")
            .attr("transform", "translate(0," + height + ")")
            .attr("class", "myXaxis");

        charts.append("g").attr("class", "myYaxis");
    }

    // Create a function that takes a dataset as input and update the plot:
    function update(data) {
        // Create the X axis:
        x.domain([
            d3.min(data, function (d) {
                return d.x;
            }),
            d3.max(data, function (d) {
                return d.x;
            }),
        ]);
        charts.selectAll(".myXaxis").transition().duration(3000).call(xAxis);

        // create the Y axis
        y.domain([
            d3.min(data, function (d) {
                return d.y;
            }),
            d3.max(data, function (d) {
                return d.y;
            }),
        ]);
        charts.selectAll(".myYaxis").transition().duration(3000).call(yAxis);

        // Create a update selection: bind to the new data
        var u = charts.selectAll(".lineTest").data([data], function (d) {
            return d.x;
        });

        // Updata the line
        u.enter()
            .append("path")
            .attr("class", "lineTest")
            .merge(u)
            .transition()
            .duration(3000)
            .attr(
                "d",
                d3
                    .line()
                    .x(function (d) {
                        return x(d.x);
                    })
                    .y(function (d) {
                        return y(d.y);
                    })
            )
            .attr("fill", "none")
            .attr("stroke", "steelblue")
            .attr("stroke-width", 2.5);
    }

    // At the beginning, run the update function on the first dataset:
    async function firstUpdate() {
        const x = await charts;
        update(todo_count_formatted);
    }
    firstUpdate();
    $: update(todo_count_formatted);
</script>

<!-- Add 2 buttons -->
<button on:click={() => update(todo_count_formatted)}>View Whole Data</button>
<button on:click={() => update(todo_count_formatted_last_5)}>View Last 5</button
>

<!-- Create a div where the graph will take place -->
<div bind:this={my_dataviz} class="chart" />

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

    button {
        font-family: "Nunito", sans-serif;
        font-weight: 300;
        line-height: 2;
    }
</style>
