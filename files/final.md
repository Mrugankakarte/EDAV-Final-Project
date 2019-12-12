---
date: "12/12/2019"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Interactive stuff

<div id="chart" style="width: 400px; float: left;">
  <h3>Graph</h3>
  <p>Click a point to show its summary stats</p>
</div>

<div id="summary" style="width: 400px; float: left;">
  <h3>Summary Stats for selected region</h3>
</div>

<script src="https://d3js.org/d3.v5.js"></script>

<script>
  //TODO

  //Title
  //Axis Labels x2
  //Make circle radii in proportion to num_migrations

  CSV_SOURCE = "http://127.0.0.1:8080/summary.csv";
  OPACITY_LEVEL = 0.7;
  DOTRADIUS = 4.5;

  d3.select("#chart").append("form").attr("id", "selection");
  
  d3.select("form").append("input")
    .attr("type", "radio")
    .attr("id", "terrainradio")
    .attr("name", "mode");

  d3.select("form").append("label")
    .attr("for", "terrainradio")
    .text(" Terrain Rendering ");

  d3.select("form").append("input")
    .attr("type", "radio")
    .attr("id", "watercolorradio")
    .attr("name", "mode");

  d3.select("form").append("label")
    .attr("for", "watercolorradio")
    .text(" Watercolor rendering ");

  var bottomLat = 41.810;
  var topLat = 43.285;
  var leftLong = 19.995;
  var rightLong = 21.797;

  var w = 450;
  var h = 450;

  //I adjusted these carefully to make sure the axes 'snug-ly' wrap around the map image
  var margin = {top: 41, right: 59, bottom: 41, left: 59};
  var innerWidth = w - margin.left - margin.right;
  var innerHeight = h - margin.top - margin.bottom;

  d3.select("#chart").attr("style", `width: ${w}px; float: left;`)

  //Create SVG
  var svg = d3.select("#chart")
    .append("svg")
      .attr("width", w)
      .attr("height", h);

  var terrainimage = svg
    .append("image")
      .attr("id", "terrainimage")
      .attr("width", innerWidth)
      .attr("xlink:href", "terrain.jpeg")
      .attr("height", innerHeight)
      .attr("x", margin.left)
      .attr("y", margin.top);

  var watercolorimage = svg
    .append("image")
      .attr("id", "watercolorimage")
      .attr("width", innerWidth)
      .attr("xlink:href", "watercolor.jpeg")
      .attr("height", innerHeight)
      .attr("x", margin.left)
      .attr("y", margin.top);

  d3.select("#terrainradio").on("click", function(){
    d3.select("#terrainimage").attr("opacity", OPACITY_LEVEL);
    d3.select("#watercolorimage").attr("opacity", 0);
  })

  d3.select("#watercolorradio").on("click", function(){
    d3.select("#terrainimage").attr("opacity", 0);
    d3.select("#watercolorimage").attr("opacity", OPACITY_LEVEL);
  })

  var xScale = d3.scaleLinear()
    .domain([leftLong, rightLong])
    .range([0, innerWidth]);

  var yScale = d3.scaleLinear()
    .domain([bottomLat, topLat])
    .range([innerHeight, 0]); //Inverting it since y starts at top

  var xAxis = d3.axisBottom(xScale);
  var yAxis = d3.axisLeft(yScale);

  svg.append("g")
   .attr("class", "yAxis")
   .attr("transform", `translate(${margin.left},${margin.top})`)
   .call(yAxis);

  svg.append("g")
   .attr("class", "xAxis")
   .attr("transform", `translate(${margin.left},${h-margin.bottom})`)
   .call(xAxis);

  seconddiv = d3.select("#summary");
  seconddiv.append("p").text("Municipality ID: ")
    .attr("id", "mcode")
  seconddiv.append("p").text("Total Migrations: ")
    .attr("id", "migrations");
  seconddiv.append("p").text("Total Killings: ")
    .attr("id", "killings");
  seconddiv.append("p").text("NATO Airstrikes: ")
    .attr("id", "nato");
  seconddiv.append("p").text("Reports of KLA causalties: ")
    .attr("id", "kla");

  function updateStats(id, summary){
    d3.select("#mcode").text("Municipality ID: " + id);
    d3.select("#migrations").text("Total Migrations: " + summary.migrations);
    d3.select("#killings").text("Total Killings: " + summary.killings);
    d3.select("#nato").text("NATO Airstrikes: " + summary.nato_airstrikes);
    d3.select("#kla").text("Reports of KLA causalties: " + summary.kla);
  }

  //DATA
  d3.csv(CSV_SOURCE, function(d) {
    return {
      mcode : d["mcode"],
      killings : +d["total_killings"],
      migrations : +d["total_migrations"],
      nato_airstrikes : +d["nato_airstrikes"],
      kla : +d["num_kla_events"],
      lat : +d["mean_latitude"],
      long : +d["mean_longitude"]
    };
  }).then(function(data){
    var summary = {};
    for (i = 0; i < data.length; i++){
      //Add points for each data
      row = data[i];
      if (!isNaN(row.long) && !isNaN(row.lat)){
        d3.select("svg").append("circle")
          .attr("cx", xScale(row.long))
          .attr("cy", yScale(row.lat))
          .attr("r", DOTRADIUS)
          .attr("id", row.mcode);
        summary[row.mcode] = {
          killings: row.killings,
          migrations : row.migrations,
          nato_airstrikes : row.nato_airstrikes,
          kla : row.kla
        };
      }
    }
    //Add event listeners
    d3.selectAll("circle").on("click", function(){
      d3.selectAll("circle").attr("fill","black");
      d3.selectAll("circle").attr("r",DOTRADIUS);
      var circle = d3.select(this);
      circle.attr("fill", "#BF3EFF");
      oldR = circle.attr("r");
      circle.transition().duration(350).attr("r", oldR*1.75);
      var id = circle.attr("id");
      updateStats(id, summary[id]);
    })
  }).catch(function(error){
    console.log("ERROR");
    console.log(error);
  });

  document.getElementById("terrainradio").click();

</script>