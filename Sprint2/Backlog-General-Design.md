

 - Use case 1: Visualization of lines committed per user
 	- User can navigate to "Lines" tab in augur
	- Graph is shown with data on which users have committed the most lines of code for the project
	- Graph pulls data and is populated 
 - Use case 2: Drill-down graph to show more time-specific commit data
 	- User can press button on the previously described graph to show loc/week
	- Graph pulls the same correct graph data shown but with tighter time restriction
	- User can navigate back to original graph using an exit button
	- Graph refreshes and shows original timed data
 - Use case 3: Detect backend loading failure w/ error code 
 	- User receives error code that backend has failed to load when launching augur
	- Failed backend load is detected and sends an alert box to the view
	- Eliminates confusion when stuck at loading screen 

  ## User Interface Files
![Shows created Lines tab running on our instance](https://lh3.googleusercontent.com/OlLjII_RTg9a4LC0kpiRXnj3TRE0u-jJcHZlOulMEI5D5n-wcYeK7w4m9kXY_b83FdqU6At7jfPE)

## Use Case Mock up:
[http://ec2-13-58-174-130.us-east-2.compute.amazonaws.com:3333/single/github.com%2Ftwitter%2Ftwemoji/lines#](http://ec2-13-58-174-130.us-east-2.compute.amazonaws.com:3333/single/github.com%2Ftwitter%2Ftwemoji/lines#)


Use Case 1 and 2 is deployed here. You can see use case 1's graph marked "Commits of code added by the top 10 authors as Percentages- By Time Period". However use case 2 is being a little more subborn while we do have a general outline of our graph, but we have not got the data to appear the way we want.

-Starting form the launch page of Augur you click your repository.

![enter image description here](https://lh3.googleusercontent.com/Is36rK1WLBvRoPfqJGa5phHm67XDRKJ130ufO2u6niVBEBE3a3Qs1NW4XtrmoNrtkXoob-5sSfw "pick your git")

-Next select the tab "lines"

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
	In order to do this a new Sql command had to be made in the "augur/datasources/facade.py" file. This SQL command is read in "augur/datasources/facade/routes.py". The command issued in "routes.py" to the augur database, the database returns the requested information is tagged and can be requested from the frontend. The frontend uses frontend/app/AugurAPI.js to map the tag to any page that calls it. In our case we created "commitsByAuthor". commitsByAuthor is then called by our new chart "frontend/app/components/charts/CommitsNormalizedStackedBarChart.vue". This chart is then called by our page "frontend/app/components/LinesCard.vue". This card is called by any page running the "frontend/app/components/Tabs.vue" which is how our card is called via frontend/app/router/router.js.
"router.js" is where almost all the pages are called and displayed.

Use Case 2:	

Use case 2 is taking a little more time than anticipated. The problem our team has ran into is creating a new graph from scratch.  We are new to using "Vega". We have been working through a lot of trial and error. One major set back is the time to reset the local instance when trouble shooting. We did try to use the ec2 instance however it also has issues. The ec2 instance, when faulted by bad code, leaves several threads running in the background. These threads must be shutdown manually before the instance can be restored.

The over all structure from database to web page is generally the same. We intend to use the GitEndpoint we created for use case 1, GitEndpoint(repo, 'commitsByAuthor', 'commits_by_author'). The mapping of this data is the same as above. 
	
In order to do this a new Sql command had to be made in the "augur/datasources/facade.py" file. This SQL command is read in "augur/datasources/facade/routes.py". The command issued in "augur/datasources/facade/routes.py" to the augur database, the database returns the requested information is tagged and can be requested from the frontend. The frontend uses AugurAPI.js to map the tag to any page that calls it. In our case we created "commitsByAuthor". 
	
This is where use case differs. CommitsByAuthor is now going to be called by our new chart "frontend/app/components/charts/ZoomChart.vue". This chart is then called by our page "frontend/app/components/LinesCard.vue". This card is called by any page running the "frontend/app/components/Tabs.vue" which is how our card is called via frontend/app/router/router.js.
"router.js" is where almost all the pages are called and displayed.

Use Case 3:
	
Use case 3 was abandoned. There was a conversations with the stakeholder. The reason for the abandonment was while the idea was sound, there was no good application. The original intention was to indicate the error of not including the Git Hub API key in augur.config.json. The problem we ran into while trying to apply the use case is if on an ec2 instance you would never see the web page the issue causes. The first step of augur is to access this key to retrieve the informatio from the augur database. The whole of augur crashed without the key thus you can not notify anyone via the web page generated by augur. The second senario is if we run augur on a local host. When you run augur on local host it is continually using up dataprocessing time and throwing up errors in the command line where it is launched. So between the fact you can already see an error and augur is looping in the command line using up computing power. You should just shut it down while you have the command line up. 
The logic augur uses the augur.config.json in is the augur/applicaiton.py file.


## Databases Schema

We are making use of the analysis_data schema in our data mining.

![enter image description here](https://lh3.googleusercontent.com/BSjtrzwxacR2XowM64OlyCwYPRZeX11RdOiQiIXrRIw38sQDGiT1dLjTEA6Uuibe9cdU-euCPgU "schema")

When calling the commits-changed-by-author we used this SQL command to generate the information used in our graphs.

![enter image description here](https://lh3.googleusercontent.com/bLMmw17aKzgprtOsA_gOZ9vsfJWfSeX0x2L11Jth2R5wEkIn8qi_TIc-5-qnya5DvUPrClhU8Xo "commits sql")

## Bonus

We also ran pytest on our database pull while they did come back as errored. It was mainly due to other configuration.

![enter image description here](https://lh3.googleusercontent.com/8lq6XUbCacsW2yyqDvr4WmHMoA4GpS9V9On6b4i07xl2_QPZjYfbGOrGwoA5b1W9RhF68EVvAJI "bonus 2")

![enter image description here](https://lh3.googleusercontent.com/V8cETGISeTpffvpxsWeIeKARg7zrIMH7KD4E-7PTT3NPlXH2UusDsCOgkptZ1l1LPLuBKo-cvZo "bonus 1")
