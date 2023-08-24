<!DOCTYPE html>
<head>
  <meta charset="utf-8">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.5/d3.min.js"></script>
  <style>
    body { margin:0;position:fixed;top:0;right:0;bottom:0;left:0; }
    svg { width: 100%; height: 100%; }
  </style>
</head>

<body>
  <script>
    var margin = {top: 20, right: 10, bottom: 20, left: 10};
    var width = 960 - margin.left - margin.right;
    var height = 500 - margin.top - margin.bottom;
    var circleRadius = 20;
    var delay = 100; // in ms
    var circleOffset = circleRadius;
    
    var svg = d3.select("body").append("svg")
      .attr("width", width + margin.left + margin.right)
      .attr("height", height + margin.top + margin.bottom)
    .append("g")
      .attr("transform", "translate(" + margin.left + "," + margin.top + ")");
    
    var delays = [1, 5, 10, 20, 50, 100, 500, 1000, 2000];
    
    var circle2 = svg.append('circle')
    	.attr('cx', (width/2)-circleOffset)
    	.attr('cy', (height/2)-circleOffset)
    	.attr('r', circleRadius)
    	.attr('fill', 'red')
    
    var circle1 = svg.append('circle')
    	.attr('cx', (width/2)-circleOffset)
    	.attr('cy', (height/2)-circleOffset)
    	.attr('r', circleRadius)
    	.attr('fill', 'gray')
    
    var ds = svg.selectAll('.delay')
    		.data(delays)
    	.enter()
    		.append('g')
    			.attr('class', 'delay')
    			.attr('left', 100)
    			.attr('top', 50)
    			.attr('transform', function(d, i) { return 'translate(' + i*100 + ', 0)'; })
    
    function updateButtons() {
      svg.selectAll('.delay')
        .select('text')
      	.attr('fill', function(d) { return d === delay ? 'black' : 'lightblue' })
    }
    
    ds.append('text')
      .text(function(d) { return d + ' ms'; })
    	.on('click', function(d) {
      	delay = d;
      	updateButtons();
    	});
    
    svg.append('rect')
      .attr('height', height)
    	.attr('width', width)
    	.attr('opacity', 0) 
      .on('mousemove', function() {
        var start = performance.now();
        var x = d3.event.x;
        var y = d3.event.y;
    		circle1.attr('cx', x - circleOffset/2)
        circle1.attr('cy', y - circleOffset)
        
				var step = function(timestamp) {
  				if (timestamp-start < delay) {
    				window.requestAnimationFrame(step);
  				} else {
            circle2.attr('cx', x - circleOffset/2);
          	circle2.attr('cy', y - circleOffset);
          }
        }
        
        window.requestAnimationFrame(step);
    	})
    
    updateButtons();
    
  </script>
</body>
