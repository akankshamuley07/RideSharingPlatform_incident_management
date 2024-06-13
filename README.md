During a ride, if there is any unfortunate incident like accident or quarrel with a fellow passenger, etc, then the services provided by this module can be availed.
1.	It will allow the users to create a new incident
2.	Track an incident
3.	Update the investigation details into the system
Stage: Database Implementation
1.	Design a data base as per the following ER diagram provided.
 
Figure 6 : ER Diagram - Transportation Service
2.	Enfore the following constraints along with primary and foreign keys
a.	Report date should not be a future date
b.	Incident date can be today or a past date only
c.	Status for incident should be pending/closed. 
d.	Pending should be the default value for status.
Note: Feed the data into the IncidentTypes table when the application starts
Stage: Data Access Layer Design
1.	Create a library project and add ORM support into it. 
2.	Use the ORM to map the entities to database as per the ER diagram provided. 
3.	Use repository per entity pattern and generate the repositories to perform the following operations
a.	Return the available incident types
b.	Insert a new incident
c.	Return all pending incidents
d.	Return an incident by id
e.	Update an incident details

Stage: Business Logic Layer Development

1.	Develop a library which reference the Data Access Library project created earlier
2.	This class library will contain various service classes which will encapsulate the business logic for the application.
3.	Use dependency injection to in service classes to inject the required repositories.
4.	Create the service classes following the single responsibility principle which perform the given operations as follows 
a.	Get the incident types
b.	Calculate the ResolutionETA
c.	Add new incident
d.	Get pending incidents
e.	Get incident by Id
f.	Update incident report
5.	Following business rules must be implemented as part of the business service class
a.	When a new incident is created an Incident ID should be auto-generated in the format as – xxxx-xxxx. The first 4 letters should be year of incident followed by 4 digit unique number
b.	ResolutionETA should be auto-calculated
c.	A user should only be allowed to report an incident within 2 days of the actual incident. If a user tries to report an incident after 2 days, raise a user-defined exception as “CannotReportIncidentException”.
d.	A user cannot file more than 1 incident for a given booking id.
Stage: Unit Testing
1.	Create a new Unit test project to test the service classes created in business logic layers
2.	Mock all the repositories using a mocking framework.

Stage: Micro-service implementation
1.	Create a API project which references the business logic layer created earlier
2.	Implement service documentation using swagger
3.	Create the following end-points and test them using postman and export the requests into a json file.
Table 20 : Incident Management - End point  - 1
URL	/api/incidents/types
Request Type	GET
User Role	Registered users
Trigger	Front end
Description	A user should be able to choose an incident type while filing a new incident report
Inputs	
Outputs	IncidentTypeDTOs

Table 21 : Incident Management - End point  - 2
URL	/api/incidents/report
Request Type	POST
User Role	Registered users
Trigger	Front end
Description	Any registered user of the system should be able to report an incident happened during their ride
Inputs	NewIncidentDTO
Outputs	Status code and incident id
Table 22 : Incident Management - End point  - 3
URL	/api/incidents
Request Type	GET
User Role	Security Head
Trigger	Front end
Description	A security head will use this endpoint to fetch all the pending incident from the system sorted in order of incident report date
Inputs	
Outputs	IncidentsDTOs
Table 23 : Incident Management - End point  - 4
URL	/api/incidents/<id>
Request Type	GET
User Role	Registered users
Trigger	Font end
Description	A registered user who has an incident id should be able to track the incident details. At the same time a security head may also access the incident details to file the investigation report
Inputs	IncidentId
Outputs	IncidentDTO
Table 24 : Incident Management - End point  - 5
URL	/api/incidents/<id>/investigationreport
Request Type	PUT
User Role	Security head
Trigger	Font end
Description	The security head in the system will use this end point to provide the investigation report and close a given incident
Inputs	IncidentId and InvestigationReportDTO
Outputs	Status code


Stage: Font-end design

Create the following components as per the specification provided below. 
1.	NewIncidentComponent
a.	Create a component and allow the navigation to it for motorists and riders 
b.	The component should provide a form for the users to report an incident
c.	Use a dropdown to list the types of incidents
d.	User a multi-line textbox for the incident details65
e.	Validate all the data before it’s submitted
f.	On successful submission of incident display the auto-generated incident id

2.	IncidentsListComponent
a.	Create a component which is accessible to security heads
b.	The component should display the pending incidents in a tabular formats
c.	The security heads should be able to sort the incidents based on incident type and incident dates
d.	Each incident should have view details button which should navigate to track incident.

3.	TrackIncidentComponent
a.	Design a new component for application users to track an incident filed by them
b.	The component should display all the incident details along with the investigation details
c.	Provide a navigation in the application navbar to access the component
d.	If a security head is navigating to track incident component by clicking the view details button in IncidentList component than that particular incident details must be displayed
e.	Along with incident details a security head should be provided with a form to file the investigation findings

Stage: Integration of Frontend and backend
1.	Create a data service in the font-end application which will communicate with the micro services.
2.	Use the data service in the components to make them interact with the API
3.	Valid error messages should be shown based on various response status codes received form the API
