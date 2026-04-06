<script>
    // import BarHorizontal from "$lib/BarHorizontal.svelte";
    import ScatterPlot from "$lib/ScatterPlot.svelte";
    import LineChart from "$lib/LineChart.svelte";

    import { base } from '$app/paths';
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    let locData = [];
    let commits = [];
    let linesByDate = [];

    onMount(async () => {
        locData = await d3.csv(`${base}/loc.csv`, row => ({
            ...row,
            line: Number(row.line),
            depth: Number(row.depth),
            length: Number(row.length),
            date: new Date(row.date + "T00:00" + row.timezone),
            datetime: new Date(row.datetime)
        }));

        // console.log(locData);
        
        commits = d3.groups(locData, d => d.commit).map(([commit, lines]) => {
            let first = lines[0];
            let {author, date, time, timezone, datetime} = first;
            let ret = {
                id: commit,
                url: "https://github.com/allisoneto/portfolio/commit/" + commit,
                author, date, time, timezone, datetime,
                hourFrac: datetime.getHours() + datetime.getMinutes() / 60,
                totalLines: lines.length,
                lines: lines
            };

            return ret;
        });

        

        console.log(commits);
    });

    $: {const rolled = d3.rollups(
            locData, v => v.length, d => d3.timeDay.floor(d.datetime)
        ).map(([date, count]) => ({ date, count }));

        const [minDate, maxDate] = d3.extent(rolled, d => d.date);
        const allDays = d3.timeDays(minDate, d3.timeDay.offset(maxDate, 1));

        linesByDate = allDays.map(date => ({
            date, count: rolled.find(d => d.date.getTime() === date.getTime())?.count ?? 0
        }));
    }


    
    // $: barData = d3.rollups(locData, v => v.length, d => d.type)
    //     .map(([type, count]) => ({ label: String(type), value: count }));

</script>

<h1>Meta</h1>

<p>
    Meta page to visualize project data
</p>
<!-- 
<BarHorizontal data={barData} /> -->

<ScatterPlot commits={commits} locData={locData} />

<LineChart linesByDate={linesByDate}/>