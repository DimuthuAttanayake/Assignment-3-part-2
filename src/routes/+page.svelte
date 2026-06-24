<script>
  import * as d3 from 'd3'

  // 1. DATA
  const data = [
    { year: 1950, production: 2 }, { year: 1951, production: 2 }, { year: 1952, production: 2 },
    { year: 1953, production: 3 }, { year: 1954, production: 3 }, { year: 1955, production: 4 },
    { year: 1956, production: 5 }, { year: 1957, production: 5 }, { year: 1958, production: 6 },
    { year: 1959, production: 7 }, { year: 1960, production: 8 }, { year: 1961, production: 9 },
    { year: 1962, production: 11 }, { year: 1963, production: 13 }, { year: 1964, production: 15 },
    { year: 1965, production: 17 }, { year: 1966, production: 20 }, { year: 1967, production: 23 },
    { year: 1968, production: 27 }, { year: 1969, production: 32 }, { year: 1970, production: 35 },
    { year: 1971, production: 38 }, { year: 1972, production: 44 }, { year: 1973, production: 51 },
    { year: 1974, production: 52 }, { year: 1975, production: 46 }, { year: 1976, production: 54 },
    { year: 1977, production: 59 }, { year: 1978, production: 64 }, { year: 1979, production: 71 },
    { year: 1980, production: 70 }, { year: 1981, production: 72 }, { year: 1982, production: 73 },
    { year: 1983, production: 80 }, { year: 1984, production: 86 }, { year: 1985, production: 90 },
    { year: 1986, production: 96 }, { year: 1987, production: 104 }, { year: 1988, production: 110 },
    { year: 1989, production: 114 }, { year: 1990, production: 120 }, { year: 1991, production: 124 },
    { year: 1992, production: 132 }, { year: 1993, production: 137 }, { year: 1994, production: 151 },
    { year: 1995, production: 156 }, { year: 1996, production: 168 }, { year: 1997, production: 180 },
    { year: 1998, production: 188 }, { year: 1999, production: 202 }, { year: 2000, production: 213 },
    { year: 2001, production: 218 }, { year: 2002, production: 231 }, { year: 2003, production: 241 },
    { year: 2004, production: 256 }, { year: 2005, production: 263 }, { year: 2006, production: 280 },
    { year: 2007, production: 295 }, { year: 2008, production: 281 }, { year: 2009, production: 288 },
    { year: 2010, production: 313 }, { year: 2011, production: 325 }, { year: 2012, production: 338 },
    { year: 2013, production: 352 }, { year: 2014, production: 367 }, { year: 2015, production: 381 },
  ]

  // 2. DIMENSIONS
  const width = 680
  const height = 420
  const marginTop = 70
  const marginRight = 20
  const marginBottom = 55
  const marginLeft = 50

  // 3. SCALES
  const x = d3.scaleLinear()
    .domain(d3.extent(data, d => d.year))
    .range([marginLeft, width - marginRight])

  const y = d3.scaleLinear()
    .domain([0, 400])
    .range([height - marginBottom, marginTop])

  // 4. GENERATORS
  const areaGenerator = d3.area()
    .x(d => x(d.year))
    .y0(y(0))
    .y1(d => y(d.production))

  const lineGenerator = d3.line()
    .x(d => x(d.year))
    .y(d => y(d.production))

  const yTicks = y.ticks(5)
  const topTick = yTicks[yTicks.length - 1]
</script>

<svelte:head>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Inter:wght@400&display=swap" rel="stylesheet" />
</svelte:head>

<div class="wrapper">
  <h1>Global production of primary plastics have increased from <span class="black">2 to 381 million tonnes</span> in 65 years</h1>
  <p class="subtitle">Global annual production of polymer resin and fibers, 1950–2015.</p>

  <svg viewBox="0 0 {width} {height}" style="width: 100%;">
    <!-- Gridlines -->
    {#each yTicks as tick}
      <line
        x1={marginLeft}
        x2={width - marginRight}
        y1={y(tick)}
        y2={y(tick)}
        stroke="#e8e7e1"
      />
    {/each}

    <!-- Area fill + outline -->
    <path fill="#a8a7a0" d={areaGenerator(data)} />
    <path fill="none" stroke="#2c2c2a" stroke-width="1" d={lineGenerator(data)} />

    <!-- Y-axis label -->
    <text
      x={marginLeft - 48}
      y={y(topTick) - 16}
      text-anchor="start"
      class="axis-label"
    >Plastic production</text>

    <!-- Y-axis tick labels -->
    {#each yTicks as tick}
      <text
        x={marginLeft - 8}
        y={y(tick)}
        text-anchor="end"
        dominant-baseline="middle"
        class="tick-label"
      >{tick}{tick === topTick ? ' Mt' : ''}</text>
    {/each}

    <!-- X-axis tick labels -->
    {#each x.ticks(8) as tick}
      <text
        x={x(tick)}
        y={height - marginBottom + 18}
        text-anchor="middle"
        class="tick-label"
      >{tick}</text>
    {/each}

  </svg>

  <div class="footer">
    <span>Chart by: Dimuthu Attanayake</span>
    <span>Source: <a href="https://ourworldindata.org/grapher/global-plastics-production" target="_blank" rel="noopener">Geyer et al. (2017), Our World in Data</a></span>
  </div>
</div>

<style>
  .wrapper {
    max-width: 680px;
    margin: 0 auto;
    padding: 2rem 2rem 2rem 7.4%;
    font-family: 'Inter', system-ui, sans-serif;
  }

  h1 {
    font-family: 'Playfair Display', Georgia, serif;
    font-size: 28px;
    font-weight: 700;
    color: #8a8983;
    margin: 0 0 6px 0;
    line-height: 1.2;
  }

  .black {
    color: #2c2c2a;
  }

  .subtitle {
    font-family: 'Inter', system-ui, sans-serif;
    font-size: 13px;
    font-weight: 400;
    color: #999;
    margin: 0 0 8px 0;
  }

  .tick-label {
    font-family: 'Inter', system-ui, sans-serif;
    font-size: 12px;
    fill: #aaa;
  }

  .axis-label {
    font-family: 'Inter', system-ui, sans-serif;
    font-size: 12px;
    fill: #999;
  }

  .footer {
    display: flex;
    gap: 24px;
    margin-top: 2px;
    padding-left: 50px;
    font-family: 'Inter', system-ui, sans-serif;
    font-size: 10px;
    color: #aaa;
  }

  .footer a {
    color: #aaa;
    text-decoration: underline;
  }
</style>