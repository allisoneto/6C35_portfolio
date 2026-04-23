<script>
    import * as d3 from 'd3';

    let width = 400;
    let height = 200;

    export let data = [];

    let margin = { top: 40, right: 50, bottom: 30, left: 60 };

    let innerWidth = width - margin.left - margin.right;
    let innerHeight = height - margin.top - margin.bottom;

    let xAxis, yAxis;

    let selectedIndex = -1;
    let liveText = '';
    let showChart = true;

    $: xScale = d3
        .scaleBand()
        .domain(data.map((d) => d.label))
        .range([0, innerWidth])
        .padding(0.2);

    $: yScale = d3
        .scaleLinear()
        .domain([0, d3.max(data, (d) => d.value) || 1])
        .range([innerHeight, 0]);

    $: colorScale = d3
        .scaleOrdinal()
        .domain(data.map((d) => d.label))
        .range(
            data.length
                ? d3.quantize(d3.interpolateBlues, data.length)
                : ['#9ecae1']
        );

    $: if (xAxis && yAxis) {
        d3.select(xAxis).call(d3.axisBottom(xScale));
        d3.select(yAxis).call(
            d3
                .axisLeft(yScale)
                .tickFormat((d) => (Number.isInteger(d) ? d : ''))
                .tickValues(d3.range(0, d3.max(data, (d) => d.value) + 1))
        );
    }

    $: maxBar = d3.greatest(data, (d) => d.value);

    $: description =
        data.length > 0
            ? `Project counts by year: ${data.map((d) => `${d.label}, ${d.value}`).join('; ')}.`
            : 'No project data for this chart yet.';

    function toggleBar(index, event) {
        const key = event.key;
        if (key && key !== 'Enter' && key !== ' ') return;
        selectedIndex = index;
        const d = data[index];
        if (d) {
            const n = d.value;
            liveText = `${d.label}: ${n} project${n === 1 ? '' : 's'} selected.`;
        }
    }

    /** Activate bar from keyboard (Enter / Space) on keydown — matches typical button behavior. */
    function onBarKeydown(index, event) {
        if (event.key !== ' ' && event.key !== 'Enter') return;
        event.preventDefault();
        toggleBar(index, event);
    }

    function toggleView() {
        showChart = !showChart;
        liveText = showChart ? 'Bar chart view shown.' : 'Table view shown.';
    }
</script>

<button
    type="button"
    on:click={toggleView}
    aria-pressed={!showChart}
    aria-label="Toggle between bar chart and table view"
    class="toggle-button">
    {showChart ? 'Show table' : 'Show chart'}
</button>

<p aria-live="polite" class="sr-only">{liveText}</p>

{#if showChart}
    <div class="container">
        <!-- No role="img" on root SVG: it hides interactive descendants from AT. Name/desc come from native title/desc. -->
        <svg
            viewBox="0 0 {width} {height}"
            aria-labelledby="bar-title"
            aria-describedby="bar-desc">
            <title id="bar-title">Projects per year</title>
            <desc id="bar-desc">{description}</desc>

            <!-- Visible title only; screen readers already get the same from <title> above. -->
            <text
                x={margin.left + innerWidth / 2}
                y={margin.top / 2}
                text-anchor="middle"
                class="chart-title"
                aria-hidden="true">
                Projects per year
            </text>

            <g transform="translate({margin.left}, {margin.top + innerHeight})" bind:this={xAxis} />
            <g transform="translate({margin.left}, {margin.top})" bind:this={yAxis} />

            <text
                x={margin.left + innerWidth / 2}
                y={margin.top + innerHeight + margin.bottom + 10}
                text-anchor="middle"
                class="axis-label">
                Year
            </text>

            <text
                x={-(margin.top + innerHeight / 2)}
                y={margin.left - 40}
                text-anchor="middle"
                transform="rotate(-90)"
                class="axis-label">
                Number of Projects
            </text>

            <g transform="translate({margin.left}, {margin.top})" class="bars-layer">
                {#each data as d, index (d.label)}
                    <rect
                        class="bar"
                        x={xScale(d.label)}
                        y={yScale(d.value)}
                        width={xScale.bandwidth()}
                        height={innerHeight - yScale(d.value)}
                        fill={colorScale(d.label)}
                        stroke="black"
                        stroke-width="1"
                        opacity={selectedIndex === -1 || selectedIndex === index ? 1 : 0.45}
                        tabindex="0"
                        role="button"
                        aria-label="Year {d.label}, {d.value} project{d.value === 1 ? '' : 's'}"
                        on:click={(e) => toggleBar(index, e)}
                        on:keydown={(e) => onBarKeydown(index, e)} />
                {/each}
                {#if maxBar}
                    <rect
                        class="max-marker"
                        x={xScale(maxBar.label)}
                        y={yScale(maxBar.value)}
                        width={xScale.bandwidth()}
                        height={innerHeight - yScale(maxBar.value)}
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2" />
                    <line
                        class="max-marker"
                        x1={xScale(maxBar.label) + xScale.bandwidth()}
                        y1={yScale(maxBar.value) + (innerHeight - yScale(maxBar.value)) / 2}
                        x2={xScale(maxBar.label) + xScale.bandwidth() + 30}
                        y2={yScale(maxBar.value) + (innerHeight - yScale(maxBar.value)) / 2}
                        stroke="currentColor"
                        stroke-width="1" />
                    <text
                        class="max-marker annotation"
                        x={xScale(maxBar.label) + xScale.bandwidth() + 35}
                        y={yScale(maxBar.value) + (innerHeight - yScale(maxBar.value)) / 2}
                        dominant-baseline="middle">
                        Year with most projects
                    </text>
                {/if}
            </g>
        </svg>

        <ul class="legend">
            {#each data as d (d.label)}
                <li style="--color: {colorScale(d.label)}">
                    <span class="swatch"></span>
                    {d.label} <em>({d.value})</em>
                </li>
            {/each}
        </ul>
    </div>
{:else}
    <table aria-label="Table showing project counts by year" class="data-table">
        <caption>Projects by year</caption>
        <thead>
            <tr>
                <th id="year-header" scope="col">Year</th>
                <th id="projects-header" scope="col">Projects</th>
            </tr>
        </thead>
        <tbody>
            {#each data as d, i (d.label)}
                <tr>
                    <th id="row-{i}" scope="row">{d.label}</th>
                    <td aria-labelledby="row-{i} projects-header">{d.value}</td>
                </tr>
            {/each}
        </tbody>
    </table>
{/if}

<style>
    .toggle-button {
        margin-top: 0.5rem;
        padding: 0.35rem 0.75rem;
        cursor: pointer;
        font: inherit;
    }

    .toggle-button:focus-visible {
        outline: 2px solid #1f77b4;
        outline-offset: 2px;
    }

    .sr-only {
        position: absolute;
        left: -9999px;
        width: 1px;
        height: 1px;
        overflow: hidden;
    }

    svg {
        max-width: 100%;
        height: auto;
        overflow: visible;
    }

    .chart-title {
        font-size: 1em;
        font-weight: bold;
        fill: currentColor;
    }

    .axis-label {
        font-size: 0.8em;
        fill: currentColor;
    }

    .annotation {
        font-size: 0.7em;
        fill: currentColor;
        font-style: italic;
    }

    .container {
        display: flex;
        gap: 1.5rem;
        margin-top: 1.5rem;
        width: 100%;
    }

    .bar {
        transition:
            opacity 200ms,
            stroke 200ms,
            stroke-width 200ms;
        outline: none;
        cursor: pointer;
    }

    .bar:hover {
        stroke-width: 2px;
    }

    .bar:focus-visible {
        stroke: #fff;
        stroke-width: 2px;
        stroke-dasharray: 4;
    }

    .max-marker {
        pointer-events: none;
    }

    .legend {
        flex: 1;
        list-style: none;
        padding: 0;
        margin: 0;
    }

    .legend li {
        display: flex;
        align-items: center;
        gap: 0.5em;
    }

    .swatch {
        width: 1em;
        height: 1em;
        background: var(--color);
        margin-right: 0.5em;
        border: 1px solid #222;
        border-radius: 3px;
    }

    .data-table {
        margin-top: 1rem;
        margin-bottom: 1rem;
        border-collapse: collapse;
        width: 100%;
        max-width: 30em;
    }

    .data-table caption {
        font-weight: bold;
        margin-bottom: 0.5em;
        text-align: left;
    }

    .data-table th,
    .data-table td {
        border: 1px solid #ccc;
        padding: 0.5em;
        text-align: left;
    }

    .data-table th {
        background-color: #f0f0f0;
    }
</style>
