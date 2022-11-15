# freightways_tq
Tech Question
The following tests are a representation of REST API testing utilising Postman.
NB - The resource will not be really created, updated or deleted on the server but it will be faked as if.

Why Postman?

Postman is an easy to use API testing tool. User friendly in relation to creating, sending and examining responses. Therefore being popular when it comes to manual and exploratory testing of APIs. Easy to use for those not too familiar with Javascript as it has a a number of code snippets available.


REST API

The API tested here is the JSON Placeholder API, a free fake API for testing and prototyping. The API has six endpoints across a number of different types:
* /posts
* /comments
* /albums
* /photos
* /todos
* /users

Postman Collection

The Postman collection is a JSON structure containing all the API requests and the associated tests. The collection itself is hierarchical, running in order of purpose:
* collection
* requests (with example sub requests)
Each level in this hierarchy supports pre-request scripts and tests. Both are points at which JavaScript code can be entered with pre-request scripts run before the request is sent (as the name suggests) and tests run afterwards. We can exploit both of these to help with our tests.

Postman Environment

A postman environment could have been created in order to switch between different configurations such as test and production, however this was beyond the scope. (I do not have enough knowledge of this yet either, however know it’s possible).
Variables can be stored too, which may include those used between environments or “temporary’ variables declared in the pre request scripts.

Variables

Postman collections and environments support the declaration and storage of variables. These are available to all requests and any code within the collection, but are not accessible outside the collection. In this case there are 6 declared variables as provided by the question:
* postsEndpoint = /posts
* commentsEndpoint = /comments
* albumsEndpoint = /albums
* photosEndpoint = /photos
* todosEndpoint = /todos
* usersEndpoint = /users
I have declared one permanent variable in the environment file with relation to posts as it is the only id use. However in practice this could create too much work as the post variable has now been hot coded to the URL end point:
* url = https://jsonplaceholder.typicode.com/posts
These variables may be combined, e.g., the URI for a request can be set to {{url}}{{postsEndpoint}} rather than https://jsonplaceholder.typicode.com/posts. That way if the base URL or endpoint changes we don't need to go through all the requests changing the URI and can just change the value of the variable.

Structure

The Postman collection is one file that runs in order of need. In other words are per the required outcome the test runs - (View, Find one entry, Create new Entry etc …)
As an example, a GET request to the /posts endpoint is expected to return a 200 OK status code as part of the response. This is a positive test of the API i.e. checking it behaves as expected and 200 is a status code associated with a correct response. 
Within the first two commands they each have a sub folder representing a negative test. In the event the incorrect information is passed the outcome will fail.
In some cases it is necessary to introduce further structure to the project with sub-folders added below the status code folders. This may be required when the schema of the responses may differ even though the status code is the same e.g. separating responses that return an array of results from those that return a single response. This is all to simplify the actual test code.

Tests

Within the testing environment when dealing with larger tests the principle of DRY (don’t repeat yourself) should be adopted. In this instance it is not a major issue, however should you expand on it, it should be a consideration. (Due to my experince with Postman I was not able to implement this, but will look into it further).
For each instance we could run the following tests:
* response is received in a timely manner
* response has the right status code
* response is in the correct format (e.g JSON)
* response schema is correct
* response body contents are correct
In order to avoid doing these tests at each request a project structure could be used by using the pre-request scripts.

Workflow Tests

In this project only a single workflow test has been implemented - This can be described according to the question presented. One can obtain the comments for a given post ID via the {BaseURL} endpoint by appending the ID request to the URL.

Workflow Structure

Get All Info

The GET request is used to call the entire dataset. A copy of the data set is made into a variable within the pre request script which could be used part of the project. Which indirectly means that a pre-request script will apply to all the requests that are part of that collection. However this has not been expanded on as it’s not required for this question.
The TESTS above have been run on this initial instance.
We verify the response has a status code of 200, that the response is in JSON format, that the response is returned in a  the schema is correct, the results are an array (with at least 1 element) and finally that the response body contents are as expected.

Get Single Item

There is no pre-request script. The purpose of this test is to call a specific piece of data from the dataset by calling an ID using the GET command.
This request runs the tests defined within the folder under the tests tab.
We verify the response has a status code of 200, that the response is in JSON format (content type), received in a timely manner and finally that the response body contents are as expected.
The sub folder also include two examples in the form of requesting another ID as well as a non existent ID.

Creating a Resource

Using a POST request and by defining its structure in the body tab, we will create a new data entry point.
This request runs the tests defined within the folder under the tests tab.
We verify the response has a status code of 201, we verify the Created status code is present, that the response is in the format set out by the TEST parameters, received in a timely manner and finally that the response body contents are as expected.

Updating a Resource

Using a PUT request we will update and add the data point to the existing data set (this will be faked) under the ID of 101. We verify the response has a status code of 200, received in a timely manner and finally that the response body contents are as expected.

Deleting a resource

Using a DELETE request we will remove (in theory) the data point we just updated of ID 101. We verify the response has a status code of 200, there is a successful DELETE response, received in a timely manner and finally that the schema is correct.

Data Not Found

Using a GET request we try call a data point which does not exist in order to fullfil a Negative Test. This is achieved by calling an ID which does not exist as per the question requirements.
We verify the response has a status code of 404 in order to show Not Found and received in a timely manner.

Running the tests

The tests for each request will automatically be run when the corresponding request is sent. The tests can be run in the standard manner within Postman, i.e.
* run the entire collection
* run all tests in an individual folder
* send an individual request
The Newman command line runner can also be used.
I tried to implement the CI/CD 

CI Pipeline

