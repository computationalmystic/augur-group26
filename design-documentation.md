## Deployment Environment
[http://ec2-18-219-137-121.us-east-2.compute.amazonaws.com:3333/](http://ec2-18-219-137-121.us-east-2.compute.amazonaws.com:3333/)
## Functional Requirements

 - Use case 1: Visualization of lines committed per user
 	We had some issues with implementing this use case, and in the end 
	we had to submit an issue on the CHAOSS augur page to see about getting some insight on how to 
	get it to work.
 - Use case 2: Drill-down graph to show more time-specific commit data
 	This was a use case that we would be able to implement if our created graphs in the first 
	use case were showing up, but unfortunately we haven't been able to get it up and running. 
 - Use case 3: Detect backend loading failure w/ error code 
 	This use case ended up failing, as we wanted to stop augur if the backend failed to load and present the user an
	error message, but we were unable to find a way to execute that before the initialization.
	We're submitting another issue on the main CHAOSS repo to get some help on how to not screw everything up 
	when changing augur/application.py.

## Database Design

### ERD


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
## Languages
    1. javascript
    2. html
    3. python
	    We have nobody with prior python experience, 
	    and to bridge that gap we are mainly modifying the front end to avoid having to use it. 

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI2MzI2OTU5NSwtMTk5NjYwNjAzNl19
-->
