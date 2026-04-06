<script>
    import * as d3 from 'd3';
    import {
        computePosition,
        autoPlacement,
        offset,
    } from '@floating-ui/dom';

    import BarHorizontal from "$lib/BarHorizontal.svelte";

    let width = 1000;
    let height = 300;

    export let linesByDate = [];
    // export let locData = [];


    // let margin = { top: 20, right: 20, bottom: 30, left: 60 };
    let margin = { top: 40, right: 50, bottom: 30, left: 60 };

    let innerWidth  = width  - margin.left - margin.right;
    let innerHeight = height - margin.top  - margin.bottom;

    let usableArea = {
        top: margin.top,
        right: width - margin.right,
        bottom: height - margin.bottom,
        left: margin.left
    };
    usableArea.width = usableArea.right - usableArea.left;
    usableArea.height = usableArea.bottom - usableArea.top;

    let xAxis, yAxis;
    let yAxisGridlines;

    $: [minDate, maxDate] = d3.extent(linesByDate.map(d => d.date));
    $: maxDateFudged = new Date(maxDate);
    $: maxDateFudged.setDate(maxDateFudged.getDate() + 1);

    $: xScale = d3.scaleTime()
                .domain([minDate, maxDateFudged])
                .range([usableArea.left, usableArea.right])
                .nice();

    $: yScale = d3.scaleLinear()
                .domain([0, d3.max(linesByDate, d => d.count)])
                .range([usableArea.bottom, usableArea.top])
                .nice();
    
    $: line = d3.line()
        .x(d => xScale(d.date))
        .y(d => yScale(d.count))
        .curve(d3.curveBumpX);
    
    // $: rScale = d3.scaleSqrt()
    //             .domain([0, d3.max(commits, d => d.totalLines)])
    //             .range([2, 15]);


    // $: colorScale = d3.scaleOrdinal(d3.schemeTableau10)
    //     .domain(data.map(d => d.label));
    
    $: if (xAxis && yAxis && yAxisGridlines) {
        d3.select(xAxis).call(d3.axisBottom(xScale).ticks(8)); // reduce number of ticks
        d3.select(yAxis).call(d3.axisLeft(yScale).ticks(8));

        d3.select(yAxisGridlines).call(
            d3.axisLeft(yScale)
                .tickFormat(() => "")
                .tickSize(-usableArea.width)
        );
    }

    let hoveredDay = null; // e.g. "Monday"

    $: dayRegions = (() => {
        // if there's no data, there are no regions!
        if (linesByDate.length === 0) return [];
        return linesByDate.map((d, i, arr) => {
            // get the previous date, if it exists
            const prev = arr[i - 1];
            // get the next date, if it exists
            const next = arr[i + 1];
            // define the left side of the region, which might be the edge of the axis if this is the first date
            const left = prev ? new Date((d.date.getTime() + prev.date.getTime()) / 2) : d.date;
            // define the right side of the region, which might be the edge of the axis if this is the last date
            const right = next ? new Date((d.date.getTime() + next.date.getTime()) / 2) : d.date;
            
            return {
                date: d.date,
                weekday: d.date.toLocaleString("en", { weekday: "long" }), // e.g. "Monday"
                x: xScale(left),
                width: xScale(right) - xScale(left),
            };
        });
    })();



</script>

<div class="container">
    <svg viewBox="0 0 {width} {height}" on:mouseleave={() => hoveredDay = null}>

        <text
            x={margin.left + innerWidth / 2}
            y={margin.top / 2}
            text-anchor="middle"
            class="chart-title">
            Lines of Code by Date
        </text>

        <g class="gridlines" transform="translate({usableArea.left}, 0)" bind:this={yAxisGridlines} />
        <g transform="translate(0, {usableArea.bottom})"
            bind:this={xAxis} class="tick-label"/>
        <g transform="translate({usableArea.left}, 0)"
            bind:this={yAxis} class="tick-label"/>

        <!-- x-axis label -->
         <!-- x-axis label -->
        <text
            x={usableArea.left + (usableArea.right - usableArea.left) / 2}
            y={height + margin.bottom - 5}
            text-anchor="middle"
            class="axis-label">
            Date
        </text>

        <!-- y-axis label -->
        <text
            x={-(usableArea.top + (usableArea.bottom - usableArea.top) / 2)}
            y={10}
            text-anchor="middle"
            transform="rotate(-90)"
            class="axis-label">
            Number of Lines Edited
        </text>

        <path class="line" d={line(linesByDate)}/>


        <!-- dots at each data point -->
        {#each linesByDate as d}
            {@const isHighlighted = d.date.toLocaleString("en", { weekday: "long" }) === hoveredDay}
            <circle
                cx={xScale(d.date)}
                cy={yScale(d.count)}
                r={isHighlighted ? 5 : 3}
                fill={isHighlighted ? "var(--color-accent-high)" : "var(--color-accent-med)"}
            />
            {#if isHighlighted}
                <text
                    x={xScale(d.date)}
                    y={usableArea.top + 15}
                    text-anchor="middle"
                    font-size="12"
                    fill="var(--color-accent)"
                >
                    {Math.round(d.count)}
                </text>
            {/if}
        
        {/each}

        <!-- invisible hover regions -->
    {#each dayRegions as region}
        <rect
            x={region.x}
            y={usableArea.top}
            width={region.width}
            height={usableArea.bottom - usableArea.top}
            fill={hoveredDay === region.weekday ? "var(--color-accent-high)" : "transparent"}
            fill-opacity={hoveredDay === region.weekday ? 0.2 : 0}
            on:mouseenter={() => hoveredDay = region.weekday}
        />
    {/each}


            
    </svg>

    <!-- <dl class="info tooltip" hidden={hoveredIndex === -1} bind:this={commitTooltip} style="top: {tooltipPosition.y}px; left: {tooltipPosition.x}px">
        <dt>Commit</dt>
        <dd><a href="{ hoveredCommit.url }" target="_blank">{ hoveredCommit.id }</a></dd>
    
        <dt>Date</dt>
        <dd>{ hoveredCommit.datetime?.toLocaleString("en", {dateStyle: "full"}) }</dd>
    
        <dt>Time</dt>
        <dd>{ hoveredCommit.datetime?.toLocaleTimeString("en") }</dd>
    
        <dt>Author</dt>
        <dd>{ hoveredCommit.author }</dd>
    
        <dt>Lines edited</dt>
            <dd>{ hoveredCommit.totalLines }</dd>
    </dl> -->

    <!-- <ul class="legend">
        {#each data as d}
            <li style="--color: {colorScale(d.label)}">
                <span class="swatch"></span>
                {d.label} <em>({d.value})</em>
            </li>
        {/each}
    </ul> -->
</div>
<!-- <div class="container">
    <BarHorizontal
        data={barData}
        height={150}
        title={selectedCommits.length > 0 ? `Lines of Code: ${selectedCommits.length} Selected Commits` : "Lines of Code: Website Breakdown"}
    />
</div> -->

<style>
    svg {
        max-width: 100%;
        height: auto;
        overflow: visible;
    }

    .chart-title {
        font-size: 1.5em;
        font-weight: bold;
        fill: currentColor;
    }

    .axis-label {
        font-size: 1.2em;
        fill: currentColor;
    }

    .tick-label {
        font-size: 1.0em;
    }

    .gridlines {
        stroke: currentColor;
        stroke-opacity: 0.2;
        stroke-width: 0.6;
    }

    .dots {
        fill-opacity: 0.8;
    }

    .line {
        fill: none;
        stroke: var(--color-accent-med);
        stroke-width: 2;
    }
    
    .selected {
        fill: var(--color-accent);
    }

    dl.info {
        display: grid;
        grid-template-columns: max-content auto;
        gap: 0.2em 1em;
        margin: 0;
        transition-duration: 300ms;
        transition-property: opacity, visibility;

        &[hidden]:not(:hover, :focus-within) {
            opacity: 0;
            visibility: hidden;
        }
    }
    dl.info dt {
        margin: 0;
        color: #666;
        font-weight: normal;
        opacity: 0.8;
    }
    dl.info dd {
        margin: 0;
        font-weight: bold;
    }
    .tooltip {
        position: fixed;
        top: 1em;
        left: 1em;
        background: oklch(97% 0% 0 / 0.85);
        color: black;
        box-shadow: 0 4px 16px 0 oklch(0% 0 0 / 0.16);
        border-radius: 8px;
        padding: 0.8em 1.2em;
        z-index: 10;
        backdrop-filter: blur(4px);
        pointer-events: none;
    }

    .container {
        display: flex;
        gap: 1.5rem;
        margin-top: 1.5rem;
        width: 100%;
    }

    @keyframes marching-ants {
        to {
            stroke-dashoffset: -8; /* 5 + 3 */
        }
    }

    svg :global(.selection) {
        fill-opacity: 10%;
        stroke: black;
        stroke-opacity: 70%;
        stroke-dasharray: 5 3;
        animation: marching-ants 2s linear infinite;
    }


</style>