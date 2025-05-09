````markdown

# Bar Chart Visualization

This project is a dynamic bar chart visualization built using **HTML**, **CSS**, and **JavaScript** with the **D3.js** library. It uses a dataset containing U.S. GDP over time and displays the data as a bar chart with interactive tooltips.

### Features
- Displays a bar chart representing the U.S. GDP over time.
- The chart includes axes (X and Y) with appropriate labels.
- Interactive tooltips show more details when hovering over each bar.
- Data attributes (date and GDP) are linked to each bar and tooltip for easy reference.

### User Stories Fulfilled
- **User Story #1:** The chart includes a title with the `id="title"`.
- **User Story #2:** The x-axis has a corresponding `g` element with the `id="x-axis"`.
- **User Story #3:** The y-axis has a corresponding `g` element with the `id="y-axis"`.
- **User Story #4:** The axes contain multiple tick labels, each with the class `tick`.
- **User Story #5:** Each data point is represented by a rectangle (`rect`) element with the class `bar`.
- **User Story #6:** Each `.bar` has `data-date` and `data-gdp` attributes.
- **User Story #7:** The `.bar` elements' `data-date` properties match the order of the provided data.
- **User Story #8:** The `.bar` elements' `data-gdp` properties match the order of the provided data.
- **User Story #9:** The height of each `.bar` accurately represents its corresponding GDP value.
- **User Story #10:** The `.bar` elements align with the corresponding date on the x-axis.
- **User Story #11:** The `.bar` elements align with the corresponding GDP value on the y-axis.
- **User Story #12:** Hovering over a bar shows a tooltip with the corresponding `data-date` and `data-gdp`.
- **User Story #13:** The tooltip contains a `data-date` property matching the active `.bar` element.

### Technologies Used
- **HTML**: For structuring the page.
- **CSS**: For styling the chart and tooltip.
- **JavaScript**: For creating the dynamic bar chart using the D3.js library.
- **D3.js**: A powerful library used to manipulate the DOM and build the chart.

### Dataset
The project uses the U.S. GDP dataset from the following URL:
- [GDP Data](https://raw.githubusercontent.com/freeCodeCamp/ProjectReferenceData/master/GDP-data.json)

### How to Run the Project
1. Clone this repository or create a new project directory.
2. Create three files:
   - `index.html` (HTML structure)
   - `style.css` (CSS styles)
   - `script.js` (JavaScript functionality)
3. Include the **D3.js** library in your project by adding the CDN link in the `index.html` file.
4. Open the `index.html` file in your web browser to view the bar chart.

### Project Files Overview
- **index.html**: Contains the basic HTML structure for the chart and the tooltip.
- **style.css**: Provides basic styling for the chart, axes, and tooltip.
- **script.js**: Contains the D3.js code that processes the data and generates the chart.

### Example of `index.html`
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Bar Chart</title>
    <script src="https://d3js.org/d3.v7.min.js"></script>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div id="container">
      <h1 id="title">United States GDP Over Time</h1>
      <svg id="chart"></svg>
      <div id="tooltip"></div>
    </div>
    <script src="script.js"></script>
  </body>
</html>
```

### Example of `style.css`
```css
#container {
  width: 90%;
  margin: 0 auto;
  text-align: center;
}

#chart {
  width: 100%;
  height: 500px;
}

#tooltip {
  position: absolute;
  visibility: hidden;
  background-color: rgba(0, 0, 0, 0.8);
  color: white;
  padding: 5px;
  border-radius: 3px;
  font-size: 12px;
  pointer-events: none;
}
```

### Example of `script.js`
```javascript
d3.json("https://raw.githubusercontent.com/freeCodeCamp/ProjectReferenceData/master/GDP-data.json").then(function (data) {
  
  const dataset = data.data;
  const parseDate = d3.timeParse("%Y-%m-%d");
  
  const width = 900;
  const height = 500;
  const margin = { top: 50, right: 50, bottom: 100, left: 50 };
  
  const xScale = d3.scaleTime()
    .domain([d3.min(dataset, d => parseDate(d[0])), d3.max(dataset, d => parseDate(d[0]))])
    .range([margin.left, width - margin.right]);

  const yScale = d3.scaleLinear()
    .domain([0, d3.max(dataset, d => d[1])])
    .nice()
    .range([height - margin.bottom, margin.top]);

  const svg = d3.select("#chart")
    .attr("width", width)
    .attr("height", height);
  
  const xAxis = d3.axisBottom(xScale)
    .ticks(d3.timeYear.every(1));

  svg.append("g")
    .attr("id", "x-axis")
    .attr("transform", `translate(0,${height - margin.bottom})`)
    .call(xAxis);

  const yAxis = d3.axisLeft(yScale);

  svg.append("g")
    .attr("id", "y-axis")
    .attr("transform", `translate(${margin.left}, 0)`)
    .call(yAxis);
  
  const bars = svg.selectAll(".bar")
    .data(dataset)
    .enter()
    .append("rect")
    .attr("class", "bar")
    .attr("data-date", d => d[0])
    .attr("data-gdp", d => d[1])
    .attr("x", d => xScale(parseDate(d[0])))
    .attr("y", d => yScale(d[1]))
    .attr("width", (width - margin.left - margin.right) / dataset.length - 1)
    .attr("height", d => height - margin.bottom - yScale(d[1]))
    .attr("index", (d, i) => i);

  const tooltip = d3.select("#tooltip");

  bars.on("mouseover", function(event, d) {
    const [x, y] = d3.pointer(event);
    tooltip.style("visibility", "visible")
      .attr("data-date", d[0])
      .style("left", `${x + 20}px`)
      .style("top", `${y - 30}px`)
      .html(`<strong>Date:</strong> ${d[0]}<br><strong>GDP:</strong> $${d[1].toFixed(1)} Billion`);
  });

  bars.on("mouseout", function() {
    tooltip.style("visibility", "hidden");
  });

});
```

### License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
