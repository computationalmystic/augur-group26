

 - Use case 1: Visualization of lines committed per user
 	- User can navigate to "Lines" tab in augur
	- Graph is shown with data on which users have committed the most lines of code for the project
	- Graph pulls data and is populated 
  
  ## User Interface Files
![Shows created Lines tab running on our instance](https://lh3.googleusercontent.com/OlLjII_RTg9a4LC0kpiRXnj3TRE0u-jJcHZlOulMEI5D5n-wcYeK7w4m9kXY_b83FdqU6At7jfPE)

## Use Case Mock up:
[http://ec2-13-58-174-130.us-east-2.compute.amazonaws.com:3333/single/github.com%2Ftwitter%2Ftwemoji/lines#](http://ec2-13-58-174-130.us-east-2.compute.amazonaws.com:3333/single/github.com%2Ftwitter%2Ftwemoji/lines#)


Use Case 1 and 2 is deployed here. You can see use case 1's graph marked "Commits of code added by the top 10 authors as Percentages- By Time Period". However use case 2 is being a little more subborn while we do have a general outline of our graph, but we have not got the data to appear the way we want.

-Starting form the launch page of Augur you click your repository.

![enter image description here](https://lh3.googleusercontent.com/TnjKapyvt15HmngEZW4XRRG9-cvN1GGAvV1uBJK4A1vDzeOOx0wlAU-fv3uJxzSeb4wyFkhJtuNr1OcT-5N4ZTqNDzqMTftRxYzXW-3nwcX72z7EJzaDL_emGUWvzJzuAyostPs4HlxNdM1ogG1izKVrbm3Qh21H4wee5fewhNoJwPFLYVuIGZojbtMehQVPxYlRgRUTjyGklJbupDiH7RsPUMK3nvrnr7qHNRvVl1jypBkBfmAasoAyWEFQxZ_vjPFySywTb3SYv_c7KUW75ETqpWTYTq75K7JJ1XgSSDCFmytkeke-dFgLtoL2wrXc8QkRlL3IzSRUytA94vYq7vZ3Kgo0b6x9ClKlXYvc105fReuKvMV_eZp7vlq9EvWzLV6Tha5ooIbsHI_9j---CkNueZgInmp2icbzXJHKxJE4UXKlDTpZ30Aj69CcpzbZefAX2PJyExmDi7RVrvYBL9xyBalbn34CxpP-Vnbbcef1pPSKdieiWdXzzXhtkTW64LD0k-UyDrnm7Oz8OqMVkp0-dmA-Rg9cF2SrQ7yWRIUX2oxO-PVPeUd8ExDyUw3Noq2vK97ICR_ybxvrvD8QL8l-610pAiZGVAflqadYAxwt-ilWKo0ktKfa2BsAxqFlq4o1-9tlMighcnQaD-o0xui-W7v36Q=w928-h182-no)-Next select the tab "lines"

![enter image description here](https://lh3.googleusercontent.com/_A7V4mliPB9bHjx4WLSJeP46Do01AJP5Gnb40NrJAg9igjzBX2DsSF37Uq_Gjon7UfDitc7ZzLI "augur line tab")

-You are now on our page. This is a picture of the use case 1 graph.

![enter image description here](https://lh3.googleusercontent.com/d1CDBadjXATErk-gHMMIBKTzdokLD8bXJe_-d0Ykb5sr-ujSKuXCVMLfrLO5rsFkQQwsdXDQgtU "graph")

This is a picture of the use case 2 graph.

![enter image description here](https://lh3.googleusercontent.com/2-vMl2uwlS_5fcK0zxsBfQz1XTaFPPH3rfQaxSNOUU3Zvs0D2Nc-o-esWD8rp81zpCvhv5KES24 "case 2")

The following picture illustrates the connections of the data to the use case.

![enter image description here](https://lh3.googleusercontent.com/j_Z8zT5_NgBFIjbB0qSTL63Ypw_qJ41xPYwjtmGUGlXUT_1vbbmM_w7StffOU5wgKSt_TNLoCfo "updated diagram")



## Design Requirements

Use Case 1:
	Use case 1 is on schedule. We made a new GitEndpoint 
		GitEndpoint(repo, 'commitsByAuthor', 'commits_by_author'),
	In order to do this a new Sql command had to be made in the "facade.py" file. This SQL command is read in "facade/routes.py". The command issued in "routes.py" to the augur database, the database returns the requested information is tagged and can be requested from the frontend. The frontend uses AugurAPI.js to map the tag to any page that calls it. In our case we created "commitsByAuthor". commitsByAuthor is then called by our new chart "CommitsNormalizedStackedBarChart.vue". This chart is then called by our page "LinesCard.vue". This card is called by any page running the "Tabs.vue" which is how our card is called via router/router.js.
"router.js" is where almost all the pages are called and displayed.
