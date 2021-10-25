<!DOCTYPE html>
<html>

  <head>
    <meta name="2D Gaussian inference" content="a basic example for 2-D Gaussian inference">
    <title>2D Gaussian inference</title>
    <script src="https://unpkg.com/mathjs/lib/browser/math.js"></script>
    
    <script src="https://d3js.org/d3.v4.min.js"></script>
  </head>
  
  <!-- Create a div where the graph will take place -->
<div id="my_dataviz">
  <svg id="click" xmlns="http://www.w3.org/2000/svg">
      <defs>
          <g id="pointer" transform="scale(0.5)">
              <circle cx="0" cy="0" r="20" id="dragcircle" />
          </g>
      </defs>
  </svg>
</div>

<!-- Create a div where the graph will take place -->

  <body>
    <script>
    
    // ===================================================
    // Set up basic viewport options
    // ===================================================
    const vw = Math.max(document.documentElement.clientWidth || 0, window.innerWidth || 0)
    const vh = Math.max(document.documentElement.clientHeight || 0, window.innerHeight || 0)
      
    var height 	= math.min([vw,vh])*0.95; // 450
    var width 	= math.min([vw,vh])*0.95;

    var height_offset = 0.05*height
    var width_offset = 0.05*width
    
    var svg = d3.select("#click") // This selects the div
    .attr("width", width) // This defines the canvas' width
    .attr("height", height) // This defines the canvas' height
    
    var xrange 	= [-3.5,3.5];
    var yrange 	= [-3.5,3.5];

    // Create some basic 2D random variables
    
    var cov_obs 	= [[0.35,0],[0,0.35]];
    
    var mu				= [[0],[0]];
    var cov 			= [[1,0],[0,1]];
    
    var mu1				= [[-2],[2]];
    var cov1			= [[1.5,0],[0,1.5]];
    
    var mu2				= [[-2],[0]];
    var cov2			= [[0.35,0],[0,0.35]];

    var mu3				= [[-2],[-2]];
    var cov3			= [[0.05,0],[0,0.05]];
    
    
    var button_1_center 	= values_to_pixels([-2,2],'xy')
		var button_2_center 	= values_to_pixels([-2,0],'xy')
    var button_3_center 	= values_to_pixels([-2,-2],'xy')
    
    const radius 					= values_to_pixels(2,'x') - values_to_pixels(1,'x')
    var button_1_radius 	= radius*math.sqrt(cov1[0][0])
    var button_2_radius 	= radius*math.sqrt(cov2[0][0])
    var button_3_radius 	= radius*math.sqrt(cov3[0][0])
    
    


		const t = d3.transition()
    	.duration(750)
    	.ease(d3.easeLinear);
      
      x_limits = xrange
      y_limits = yrange

      window_x 	= [width*0.05,width*0.95];
      window_y 	= [height*0.05,height*0.95];
      

    // Get coordinates for the prior blob
    //mu_pos 	= values_to_pixels(mu_prior);
    //radius  = math.sqrt(cov_prior[0][0])/(xrange[1]-xrange[0])*width;

		svg.append('line')
    	.style("stroke", "grey")
    	.style("stroke-width", 3*height/450)
    	.attr("x1", values_to_pixels(-2,'x'))
    	.attr("y1", values_to_pixels(2,'y'))
    	.attr("x2", values_to_pixels(2,'x'))
    	.attr("y2", values_to_pixels(2,'y'))
      .attr("opacity", 0.75)
      .attr("id","compromise_line_1");

    // Plot the prior
    svg.append("circle")
      .attr("r", math.sqrt(cov1[0][0])/(xrange[1]-xrange[0])*width)
      .attr("cx", values_to_pixels(-2,'x'))
      .attr("cy", values_to_pixels(2,'y'))
      .attr("fill", "#fac205")
      .attr("stroke", "#666666")
      .attr("opacity", 0.75)
      .attr("stroke-width", 2.5*height/450)
      .attr("id","prior_1");
      
    // Plot the observation
    svg.append("circle")
      .attr("r", math.sqrt(cov_obs[0][0])/(xrange[1]-xrange[0])*width)
      .attr("cx", values_to_pixels(2,'x'))
      .attr("cy", values_to_pixels(2,'y'))
      .attr("fill", "#047495")
      .attr("stroke", "#666666")
      .attr("opacity", 0.75)
      .attr("stroke-width", 2.5*height/450)
      .attr("id","observation_1");
      
    // Plot the posterior
    svg.append("circle")
      .attr("r", math.sqrt(cov1[0][0])/(xrange[1]-xrange[0])*width)
      .attr("cx", values_to_pixels(-2,'x'))
      .attr("cy", values_to_pixels(2,'y'))
      .attr("fill", "#f43605")
      .attr("stroke", "#666666")
      .attr("opacity", 0.)
      .attr("stroke-width", 2.5*height/450)
      .attr("id","posterior_1");
      
      
      
      
		svg.append('line')
    	.style("stroke", "grey")
    	.style("stroke-width", 3*height/450)
    	.attr("x1", values_to_pixels(-2,'x'))
    	.attr("y1", values_to_pixels(0,'y'))
    	.attr("x2", values_to_pixels(2,'x'))
    	.attr("y2", values_to_pixels(0,'y'))
      .attr("opacity", 0.75)
      .attr("id","compromise_line_2");

    // Plot the prior
    svg.append("circle")
      .attr("r", math.sqrt(cov2[0][0])/(xrange[1]-xrange[0])*width)
      .attr("cx", values_to_pixels(-2,'x'))
      .attr("cy", values_to_pixels(0,'y'))
      .attr("fill", "#fac205")
      .attr("stroke", "#666666")
      .attr("opacity", 0.75)
      .attr("stroke-width", 2.5*height/450)
      .attr("id","prior_2");
      
    // Plot the observation
    svg.append("circle")
      .attr("r", math.sqrt(cov_obs[0][0])/(xrange[1]-xrange[0])*width)
      .attr("cx", values_to_pixels(2,'x'))
      .attr("cy", values_to_pixels(0,'y'))
      .attr("fill", "#047495")
      .attr("stroke", "#666666")
      .attr("opacity", 0.75)
      .attr("stroke-width", 2.5*height/450)
      .attr("id","observation_2");
      
   // Plot the posterior
    svg.append("circle")
      .attr("r", math.sqrt(cov2[0][0])/(xrange[1]-xrange[0])*width)
      .attr("cx", values_to_pixels(-2,'x'))
      .attr("cy", values_to_pixels(0,'y'))
      .attr("fill", "#f43605")
      .attr("stroke", "#666666")
      .attr("opacity", 0.)
      .attr("stroke-width", 2.5*height/450)
      .attr("id","posterior_2");
      
      
      
      
		svg.append('line')
    	.style("stroke", "grey")
    	.style("stroke-width", 3*height/450)
    	.attr("x1", values_to_pixels(-2,'x'))
    	.attr("y1", values_to_pixels(-2,'y'))
    	.attr("x2", values_to_pixels(2,'x'))
    	.attr("y2", values_to_pixels(-2,'y'))
      .attr("opacity", 0.75)
      .attr("id","compromise_line_3");
      
    // Plot the prior
    svg.append("circle")
      .attr("r", math.sqrt(cov3[0][0])/(xrange[1]-xrange[0])*width)
      .attr("cx", values_to_pixels(-2,'x'))
      .attr("cy", values_to_pixels(-2,'y'))
      .attr("fill", "#fac205")
      .attr("stroke", "#666666")
      .attr("opacity", 0.75)
      .attr("stroke-width", 2.5*height/450)
      .attr("id","prior_3");
      
    // Plot the observation
    svg.append("circle")
      .attr("r", math.sqrt(cov_obs[0][0])/(xrange[1]-xrange[0])*width)
      .attr("cx", values_to_pixels(2,'x'))
      .attr("cy", values_to_pixels(-2,'y'))
      .attr("fill", "#047495")
      .attr("stroke", "#666666")
      .attr("opacity", 0.75)
      .attr("stroke-width", 2.5*height/450)
      .attr("id","observation_3");
      
   // Plot the posterior
    svg.append("circle")
      .attr("r", math.sqrt(cov3[0][0])/(xrange[1]-xrange[0])*width)
      .attr("cx", values_to_pixels(-2,'x'))
      .attr("cy", values_to_pixels(-2,'y'))
      .attr("fill", "#f43605")
      .attr("stroke", "#666666")
      .attr("opacity", 0.)
      .attr("stroke-width", 2.5*height/450)
      .attr("id","posterior_3");
      
      
      
	// =========================================================================


      // On Click, we want to add data to the array and chart
      svg.on("click", function() {
      
      	// Check where the user just clicked
        var coords = d3.mouse(this);
        
        if (math.norm([coords[0]-button_1_center[0],coords[1]-button_1_center[1]]) < button_1_radius) {
        
          // Calculate posterior mean
          mu1 	= math.subtract(
            mu1,
            math.multiply(
              cov1,
              math.inv(math.add(cov1,cov_obs)),
              math.subtract(mu1,[[2],[2]])));

          // Calculate posterior covariance
          cov1 = math.subtract(
            cov1,
            math.multiply(
              cov1,
              math.inv(math.add(cov1,cov_obs)),
              cov1));

          d3.select("#posterior_1")
            .transition()
              .duration(750)
              .ease(d3.easeLinear)
            .attr("r", math.sqrt(cov1[0][0])/(xrange[1]-xrange[0])*width)
            .attr("cx", values_to_pixels(mu1[0][0],'x'))
            .attr("cy", values_to_pixels(mu1[1][0],'y'))
            .attr("opacity",0.75)
        
        } else if (math.norm([coords[0]-button_2_center[0],coords[1]-button_2_center[1]]) < button_2_radius) {
        
          // Calculate posterior mean
          mu2 	= math.subtract(
            mu2,
            math.multiply(
              cov2,
              math.inv(math.add(cov2,cov_obs)),
              math.subtract(mu2,[[2],[0]])));

          // Calculate posterior covariance
          cov2 = math.subtract(
            cov2,
            math.multiply(
              cov2,
              math.inv(math.add(cov2,cov_obs)),
              cov2));

          d3.select("#posterior_2")
            .transition()
              .duration(750)
              .ease(d3.easeLinear)
            .attr("r", math.sqrt(cov2[0][0])/(xrange[1]-xrange[0])*width)
            .attr("cx", values_to_pixels(mu2[0][0],'x'))
            .attr("cy", values_to_pixels(mu2[1][0],'y'))
            .attr("opacity",0.75)
        
        } else if (math.norm([coords[0]-button_3_center[0],coords[1]-button_3_center[1]]) < button_3_radius) {
        
          // Calculate posterior mean
          mu3 	= math.subtract(
            mu3,
            math.multiply(
              cov3,
              math.inv(math.add(cov3,cov_obs)),
              math.subtract(mu3,[[2],[-2]])));

          // Calculate posterior covariance
          cov3 = math.subtract(
            cov3,
            math.multiply(
              cov3,
              math.inv(math.add(cov3,cov_obs)),
              cov3));

          d3.select("#posterior_3")
            .transition()
              .duration(750)
              .ease(d3.easeLinear)
            .attr("r", math.sqrt(cov3[0][0])/(xrange[1]-xrange[0])*width)
            .attr("cx", values_to_pixels(mu3[0][0],'x'))
            .attr("cy", values_to_pixels(mu3[1][0],'y'))
            .attr("opacity",0.75)
        
        } else {
        
        	// Reset the prior
    			mu1				= [[-2],[2]];
    			cov1			= [[1.5,0],[0,1.5]];
    			mu2				= [[-2],[0]];
    			cov2			= [[0.35,0],[0,0.35]];
    			mu3				= [[-2],[-2]];
    			cov3			= [[0.05,0],[0,0.05]];
          
          // Reset the variables
          d3.select("#posterior_1")
            .attr("r", math.sqrt(cov1[0][0])/(xrange[1]-xrange[0])*width)
            .attr("cx", values_to_pixels(mu1[0][0],'x'))
            .attr("cy", values_to_pixels(mu1[1][0],'y'))
            .attr("opacity",0.)
          d3.select("#posterior_2")
            .attr("r", math.sqrt(cov2[0][0])/(xrange[1]-xrange[0])*width)
            .attr("cx", values_to_pixels(mu2[0][0],'x'))
            .attr("cy", values_to_pixels(mu2[1][0],'y'))
            .attr("opacity",0.)
          d3.select("#posterior_3")
            .attr("r", math.sqrt(cov3[0][0])/(xrange[1]-xrange[0])*width)
            .attr("cx", values_to_pixels(mu3[0][0],'x'))
            .attr("cy", values_to_pixels(mu3[1][0],'y'))
            .attr("opacity",0.)
        
        }
        

         
        })
    
    
    // helper function to output formatted results.
    function print(value) {
      var precision = 14;
      document.write(math.format(value, precision) + '<br>');
    }
    
    
    function pixels_to_values(coords,axis) {
    	axis = (typeof axis === 'undefined') ? 'xy' : axis;
    	if (axis == 'x') {
      	res = coords/width * (xrange[1]-xrange[0]) + xrange[0]
      } else if (axis == 'y') {
      	res = (1.-coords/height) * (yrange[1]-yrange[0]) + yrange[0]
      } else {
      	res = [coords[0]/width * (xrange[1]-xrange[0]) + xrange[0],(1.-coords[1]/height) * (yrange[1]-yrange[0]) + yrange[0]]
      }
     	return res;
    }

    function values_to_pixels(values,axis) {
    	axis = (typeof axis === 'undefined') ? 'xy' : axis;
    	if (axis == 'x') {
      	res = (values-xrange[0])/(xrange[1]-xrange[0])*width;
      } else if (axis == 'y') {
      	res = (1-(values-yrange[0])/(yrange[1]-yrange[0]))*height;
      } else {
      	res = [(values[0]-xrange[0])/(xrange[1]-xrange[0])*width,(1-(values[1]-yrange[0])/(yrange[1]-yrange[0]))*height];
      }
     	return res;
    }


    </script>
  </body>

</html>
