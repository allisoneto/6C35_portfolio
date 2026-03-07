<script>
	import { base } from '$app/paths';
</script>

<svelte:head>
	<title>6.C35 A2 - Exploratory Data Analysis</title>
	<link rel="stylesheet" href={base + '/projects/a2_style.css'} />
</svelte:head>

<header>
	<h1>
		<small>Assignment 2</small>
		Exploratory Data Analysis
	</h1>
	<p>
		<strong>Allison Eto</strong> — <em>allisone@mit.edu</em>
	</p>
</header>

<main>
	<section>
		<h2>Subtheme: Transit-Oriented Development</h2>
		<h3>Overall Analysis Questions</h3>
		<ol>
			<li>
				How much land would actually be available for TOD development?
				<ul>
					<li>According to the
						<a href="https://www.mass.gov/info-details/multi-family-zoning-requirement-for-mbta-communities">MBTA Communities Act</a>,
						rezoning and by extension TOD development should happen within "0.5 miles of a commuter rail stations, subway station,
						ferry terminal or bus station."
					</li>
				</ul>
			</li>
			<li>Which areas would be most ideal for TOD development?
				<ul>
					<li>I grew up having two bus lines within miles of my house, which made for some inconvenient transfers and roundabout routes.
						If there were multiple stops within 0.5 miles of me, I would've had so many more options.
					</li>
					<li>There are lots of areas in Boston (such as on MIT's campus, much to my chagrin) that are not a reasonable distance from a subway station,
						but are a reasonable distance from bus stations, which are slower and less useful. Perhaps subway stations should be weighted more heavily
						than bus stations.</li>
					<li>Could look at stop density across geography.</li>
				</ul>
			</li>
			<li>Which populated areas are still underserved by transit?
				<ul>
					<li>The MBTA Communities Act indicates that almost all of Massachusetts' eastern districts are served by the MBTA, but the
						commuter rail system becomes much less dense in the outer districts. Surely there are some gaps?
					</li>
					<li>
						Building commuter rail is expensive and time-consuming, and it's very likely that new population centers have developed
						that could use transit access.
					</li>
					<li>
						Perhaps, though, these new population centers are generally developed around existing transit stations.
					</li>
				</ul>
			</li>
		</ol>
	</section>

	<section>
		<h2>Data Processing Steps</h2>
		<p>
			Exploring the entries within MBTA stops dataset, we observe that each stop entry contains a limited number of columns--the primary of which
			include stop_name, stop_id, routes, and geometry. The "routes" column also contains a JSON object with multiple routes for many
			of the stops. This makes sense, as many stops serve as hubs for multiple bus routes or rapid transit lines. However, Tableau is unable to parse these
			JSON objects, so we use Python to pre-process the dataset. For each stop with multiple routes, we duplicate the stop and expand the route
			information (route_id, route_color, route_long_name, etc.) into their own columns.

			In order to answer some of our overall questions, it is useful to use the buffered stop datasets to analyze the TOD-friendly area around
			each stop. However, having the exact coordinates of each stop could also be useful. Moreover, the MBTA lines dataset could also be useful
			for data validation, to confirm that the routes approximately match our background knowledge of the MBTA. Therefore, we join all three
			datasets (stops, buffered stops, and lines) using the route_id and stop_id in Tableau, allowing us to access data across the different
			datasets.
		</p>
	</section>

	<section>
		<h2>Discoveries &amp; Insights</h2>

		<figure>
			<img src={base + '/images/project_imgs/a2_visualizations/coverage_overlap.png'} alt="">
			<figcaption>
				In order to inspect the matching of the lines and stops datasets, we can look at the overlap of route_ids between the two datasets.
				In this visualization, we can see that there are about 40 routes present in the lines dataset that are not present in the stops dataset,
				and 3 routes present in the stops dataset that are not present in the lines dataset. Inspecting the
				data with Tableau reveals that the extra lines dataset routes are rail replacement bus routes (i.e. shuttles from on T line station
				to another during construction). Meanwhile, the extra stops dataset routes are bus routes 10, 66, and 4050, and a quick online search
				confirms that these routes are indeed running. Because the shuttles are temporary operations for construction, we can decide to
				filter all shuttles out. Additionally, since the stops dataset contains the route information, the only thing we are missing
				for routes 10, 66, and 4050 is the geographic route of the line, which is likely not needed for our questions. A quick confirmation
				online confirms that these stop locations are accurate. We therefore default to using the stops dataset for our analysis.
			</figcaption>
		</figure>

		<figure>
			<img src={base + '/images/project_imgs/a2_visualizations/overview_map.png'} alt="">
			<figcaption>
				In order to gain a sense of the overall distribution of stops and lines, we can look at the spatial distribution of the stops and lines,
				color-coded by the designated MBTA colors. We can see that bus stops and routes make up the majority of the stops and lines, commuter rail
				lines stretch across the entire region, and rapid transit lines are concentrated in the central Boston area.
				Also of note is that most bus stops for outbound and inbound are separate, while rapid transit lines and commuter rail stops combine both
				the outbound and inbound. Only a select few stops, where the bus route is one-way only, have an independent bus stop.
				This means we likely need to weight bus stops at half magnitude to account for the duplication, and for the fact that a single
				independent bus stop is only half as useful as a bus stop that serves both directions. Additionally, many of the stations serve multiple routes, and so should be weighted by the number of routes they serve.
			</figcaption>
		</figure>

		<figure>
			<img src={base + '/images/project_imgs/a2_visualizations/nearby_station_histo.png'} alt="">
			<figcaption>
				We can now explore the question of which areas would be available for TOD development, which requires that a transit station be located
				within 0.5 miles of the area. To approximate this, we can create a grid of 250m x 250m cells using Python and count the number of stop buffers that contain the
				cell's center point. Cells with no nearby stations are removed to reduce data size. This histogram shows the distribution of how many stations
				are nearby each cell. As expected, there are a large number of cells with only a few stations nearby. Unexpectedly, there is a surprising number
				of cells with more than fifty stations nearby. This is likely due to the fact that many stations are located in dense urban areas, but also because
				many stations within a small area serve the same route. Stations for the same route are effectively duplicates, so we might want to count the
				number of nearby stations with a unique route to get a count more representative of actual utility.
			</figcaption>
		</figure>

		<figure>
			<img src={base + '/images/project_imgs/a2_visualizations/nearby_route_histo.png'} alt="">
			<figcaption>
				If we instead count the number of nearby routes, we see a dramatic reduction. Some cells still have more than twenty nearby routes, but we can explore
				where these are located on the map and compare them to the map of nearby stations and routes to confirm.
				Using this histogram, we can answer our first question: approximately 16000 cells, or 400 square miles, have at least one nearby station, and therefore
				are available for TOD development.
			</figcaption>
		</figure>

		<figure>
			<img src={base + '/images/project_imgs/a2_visualizations/nearby_route_map.png'} alt="">
			<figcaption>
				Visualizing the density of nearby routes on a map, we can see that the high-density areas do correspond with more nearby routes. Some routes shown
				also represent multiple routes that take the same physical path in certain areas. The density around South Station is particularly evident.
				Corridors of higher density approximately 0.5 miles wide are also visible along paths traveled by multiple routes.
				Interestingly, there are also several nontrivial areas with no nearby routes. This could be due to geography (i.e. a lake or mountain) or
				a park, but it would be interesting to inspect this.
			</figcaption>
		</figure>

		<figure>
			<img src={base + '/images/project_imgs/a2_visualizations/nearby_route_map_weighted.png'} alt="">
			<figcaption>
				A more accurate way to measure the transit utility in a region is to weight the nearby routes by their average speed. Of course, there are many
				factors that contribute to the utility of different transit, and this weighting is largely subjective, but for now it works as a rough approximation. The average speeds
				of the T routes were sourced from <a href="https://dashboard.transitmatters.org/">Transit Matters</a>. Similarly, the Silver Line speed
				was sourced from the <a href="https://www.nycsubway.org/wiki/MBTA_Silver_Line">NYC Subway</a>. The commuter rail speed was sourced from the
				<a href="https://www.mbta.com/safety/commuter-rail-safety">MBTA</a> website. The bus speed was sourced from <a href="https://itdp.org/2011/05/26/u-s-cities-reinventing-buses-as-modern-efficient-and-effective/">ITDP</a>.
				The ferry speed was sourced from <a href="https://www.ctps.org/data/calendar/htmls/2019/MPO_0620_Ferry_Inventory_Memo.html">CTPS</a>.

				After this weighting, we can see that high-utility areas stand out more--often around faster lines like the commuter rail. Funnily enough,
				MIT is also visibly a transit desert, despite its proximity to downtown.

				Using this map, we can also answer our second question: The best areas have the highest density, marked with teal or blue. There are several
				concentrations outside of downtown Boston (which is already heavily developed), and these areas might be even more desirable for future development.
			</figcaption>
		</figure>

		<figure>
			<img src={base + '/images/project_imgs/a2_visualizations/transport_holes.png'} alt="">
			<figcaption>
				Zooming into areas without nearby transportation, and pulling in satellite imagery of these areas, we can see that some of this dead zone is
				indeed natural spaces, but others are suburbs. Another analysis we could perform would be to map the average property value in these areas,
				since access to transportation plays a role in property value, but wealthier areas might have better access to cars and don't need to rely on transit.
			</figcaption>
		</figure>

		<figure>
			<img src={base + '/images/project_imgs/a2_visualizations/underserved_regions.png'} alt="">
			<figcaption>
				Expanding the map to reveal the outer districts and the commuter rail, we can answer our third question: which populated areas are still underserved
				by transit? As expected, most large population centers are clustered around the commuter rail. However, there is one notable area in the north
				that isn't served by the commuter rail or any transit. It also happens to be just past the end of an existing commuter rail line, so it's possible
				that the rail could be extended to this area.
			</figcaption>
		</figure>
	</section>

	<section>
		<h2>Summary</h2>
		<p>
			By visualizing the MBTA system's stops and lines, we can see that
			Boston is home to one of the more robust transit systems in the country, and it serves a large portion of Massachusetts, even outside the
			central Boston Area. However, access to transit is not uniformly distributed, and certain areas are much more suited to TOD development
			than others. Several regions even near to Boston are completely underserved, while several regions far from Boston are very well-served
			and could be great candidates for TOD development.
		</p>
		<p>
			The biggest lesson that I learned from completing this report is that datasets often need to be combined in order to answer interesting questions about
			correlation between variables. There were so many other questions that I would've liked to explore, but they all required additional datasets--demographic
			data, property values, etc.--that would
			have had to be processed and cross-matched to fit with the existing datasets. From a mechanical perspective, I also learned that Tableau is great for
			visualizing things very quickly, but Python is great for more complex data processing and analysis.
		</p>
	</section>
</main>

<style>
	main {
		max-width: 60rem;
		margin: 0 auto;
		padding: 1rem;
	}
</style>
