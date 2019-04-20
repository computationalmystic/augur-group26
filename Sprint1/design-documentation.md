## Deployment Environment
[http://ec2-13-58-174-130.us-east-2.compute.amazonaws.com:3333/](http://ec2-13-58-174-130.us-east-2.compute.amazonaws.com:3333/)
## Functional Requirements

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

## Database Design

### ERD
![](https://lh3.googleusercontent.com/e_hVb45EkjiCXvFiPhl-XpxZZvjkv0_JqaJlOCbdmEPwdLk1xlC-OXWeY28_HOKGCeq7kfR9Mh0p)

(Above) ER Diagram showing entities in the GHTorrent database 

~We have no provided DDL code because we built our program using the existing augur database schematic in mind. 


## User Interface Files
![Shows created Lines tab running on our instance](https://lh3.googleusercontent.com/OlLjII_RTg9a4LC0kpiRXnj3TRE0u-jJcHZlOulMEI5D5n-wcYeK7w4m9kXY_b83FdqU6At7jfPE)





## Model Files 
	Built from augur, using a vagrant instance. 
## Controller Files
	Added: frontend/app/component/LinesCard.vue 
		Modified from frontend/app/component/GitCard.vue
	Edited: frontend/app/router/router.js
		frontend/app/assets/AugurAPI.js
		frontend/app/component/Tabs.vue
### Description
	We added the LinesCard.vue file to create a new tab on the augur 
	home screen which would then show and populate a graph
	on the page. We had to edit router.js, AugurAPI.js, and Tabs.js to 
	incorporate our new tab. The Tabs.vue file was modified to 
	include our new Lines tab. AugurAPI and router.js were
	modified to support the new page.
## Languages
    1. javascript
    2. html
    3. python
	    We have nobody with prior python experience, 
	    and to bridge that gap we are mainly modifying the front end to avoid having to use it. 

