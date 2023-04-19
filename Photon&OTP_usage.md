# **Photon**

# Path
If you're accessing photon from within the VM or machine its running on, then your path for the requests will be:
- `curl "http://localhost:2322/api"`

Accessing Photon and making requests from outside the environment, you will need to adjust the path accordingly, and make the path accessable from outside the VM.
# Requests
You can make requests with the `?q` keyword for query. A basic search looks like this:
- curl "http://localhost:2322/api?q=berlin"

For the Searches to work as intented with autocempletition, using the correct language, we need several parameters in the request to show results accordingly.

The following keywords will have to be filled out based on the website front end information:
 - `&lang` for Language
 - `&lon` for longitude
 - `&lat` for latitude

Language:

We have to get the selected language from the front end, to pass to the back-end request as an argument.

For example if we have German language selected, the request should be:
- curl "http://localhost:2322/api?q=berlin&lang=de"

Location based searching, relevant results from the same area:

- If we need exact location based searching, and we access the users location, we can pass the the coordinates from the users login into the request.  
- If we only want to give relevant searches based on the selected language, we should use the general coordinates according to each country. 
Info:https://git.gerhardt.io/gi/devops/issues/42#issuecomment-597

Example for searches based on language:
```
switch language
case "de":
  longitude=10
  latitude=51
case "hu":
  longitude=19
  latitude=47
  ...
request = 'curl "http://localhost:2322/api?q=$searchQuery&lon=$l&lat=55.5123" '
 ... 
 (make the request, collect results)
```
# OpenTripPlanner
For OTP, requests require coordinates (from, to) to find a trip between the locations. 

An example request for OTP from within the environment looks like the following:
- `curl "http://localhost:8080/otp/routers/default/plan?fromPlace=48.776277,9.182863&toPlace=46.2564158,20.1508822&mode=WALK,TRANSIT&maxWalkDistance=2000&arriveBy=false&wheelchair=false&locale=en" `

If we have the previously selected search results, we can store the values of the selected destination, and pass the coordinates as variables to the OTP request.

Additional parameters for the search like `mode of transport`, or `arriveBy` can be filled out with variables that contain the settings passed from the front-end.