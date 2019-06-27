# fetch-api-tutorial
A guide to using JavaScript's simple Fetch API interface with typicode's JSON placeholder as the fake API to play with.


GET, POST, PUT, PATCH, and DELETE are five most common HTTP methods for retrieving from and sending data to a server.

We will be using this API for demonstrations, with credits to typicode on GitHub: https://jsonplaceholder.typicode.com/todos
We’ll also get our hands dirty by using JavaScript’s Fetch API to make requests to the API. Fetch API is JavaScript’s super simple built-in interface for making requests to servers. (Gone are the days of importing other interfaces!)

### Key Terms:
Resource: One data entry.
URI: The ID of a resource

The server https://jsonplaceholder.typicode.com/todos contains 200 resources. Click on the link and see it for youself.

# The GET method

The GET method is used to retrieve data from the server. This is a read only method, so it has no risk of mutating or corrupting the data. For example, if we call the get method on our API, we will get back a list of all to-do’s.

### Let’s try it

Let’s set up an HTML file that you can run locally on your browser. Create a file called index.html and add in the following code:

```html
<!DOCTYPE html>
<html>

<body>

   <h1>Get hands on with JavaScript's Fetch API</h1>
   <p>Write your requests in the script and watch the console and network logs.</p>


   <script>
       // GET retrieve all to-do's
       fetch('https://jsonplaceholder.typicode.com/todos')
           .then(response => response.json())
           .then(json => console.log(json))
           // will return all resources

       // GET retrieves the to-do with specific URI (in this case id = 5)
       fetch('https://jsonplaceholder.typicode.com/todos/5')
           .then(response => response.json())
           .then(json => console.log(json))

       /* will return this specific resource:
       
           "userId": 1,
           "id": 5,
           "title": "laboriosam mollitia et enim quasi adipisci quia provident illum",
           "completed": false
       
       */

   </script>

</body>

</html>
```
Loading this file will execute the script. Let’s look at the first request:
```javascript
       // GET retrieve all to-do's
       fetch('https://jsonplaceholder.typicode.com/todos')
           .then(response => response.json())
           .then(json => console.log(json))
           // will return all resources
```
This request will return everything that you can see on the server https://jsonplaceholder.typicode.com/todos. The response is a list of 200 resources, so I won’t show it here. But you can go to your network log to see the response. If you want to see a specific resource, just specify the URI to the URL, like so:

```javascript
       // GET retrieves the to-do with specific URI (in this case id = 5)
       fetch('https://jsonplaceholder.typicode.com/todos/5')
           .then(response => response.json())
           .then(json => console.log(json))

       /* will return this specific resource:
       
           "userId": 1,
           "id": 5,
           "title": "laboriosam mollitia et enim quasi adipisci quia provident illum",
           "completed": false
       
       */
```
The response is only one resource.

# The POST method

The POST method sends data to the server and creates a new resource. The resource it creates is subordinate to some other parent resource. When a new resource is POSTed to the parent, the API service will automatically associate the new resource by assigning it an ID (new resource URI). In short, this method is used to create a new data entry.

### Let’s try it

A POST method with Fetch API looks like this:

```javascript
       // POST adds a random id to the object sent
       fetch('https://jsonplaceholder.typicode.com/todos', {
               method: 'POST',
               body: JSON.stringify({
                   userId: 1,
                   title: "clean room",
                   completed: false
               }),
               headers: {
                   "Content-type": "application/json; charset=UTF-8"
               
           })
           .then(response => response.json())
           .then(json => console.log(json))

       /* will return
       
           "userId": 1,
           "title": "clean room",
           "completed": false,
           "id": 201
       
       */
```

Append this code to the script section of your index.html. Refresh your index.html page and watch the network log for changes.

Note that we needed to pass in the request method, body, and headers. We did not pass these in earlier for the GET method because by default these fields are configured for the GET request, but we need to specify them for all other types of requests. In the body, we assign values to the resource’s properties, stringified. Note that we do not need to assign a URI - the API will do that for us. As you can see from the response, the API assigns an id of 201 to the newly created resource. (Note: The server we are using is a placeholder service, so the server is just simulating the correct responses. No actual change is being done to the API, so don’t be confused if you head to https://jsonplaceholder.typicode.com/todos but do not find the new resource added.)


# The PUT method

The PUT method is most often used to update an existing resource. If you want to update a specific resource (which comes with a specific URI), you can call the PUT method to that resource URI with the request body containing the complete new version of the resource you are trying to update.

### Let’s try it
Here is a put method that is requesting a change of the name of the task with id 5:

```javascript
       // PUT to the resource with id = 5 to change the name of task
       fetch('https://jsonplaceholder.typicode.com/todos/5', {
               method: 'PUT',
               body: JSON.stringify({
                   userId: 1,
                   id: 5,
                   title: "hello task",
                   completed: false
               }),
               headers: {
                   "Content-type": "application/json; charset=UTF-8"
               
           })
           .then(response => response.json())
           .then(json => console.log(json))

       /* will return
       
           "userId": 1,
           "id": 5,
           "title": "hello task",
           "completed": false
       
       */
```

Once again, append this code to your index.html file and observe the network changes. Notice that we need to specify the method as PUT and we need to stringify the JSON object that we passed into the body. Note that the request URL is specifically the resource we want to change and the body contains all of the resource’s property, whether or not all properties need to be changed. The response will be the new version of the resource. (Note again that it is a simulated response.)


# The PATCH method

The PATCH method is very similar to the PUT method because it also modifies an existing resource. The difference is that for the PUT method, the request body contains the complete new version, whereas for the PATCH method, the request body only needs to contain the specific changes to the resource, specifically a set of instructions describing how that resource should be changed, and the API service will create a new version according to that instruction.

### Let’s try it

```javascript
       // PATCH to the resource id = 1
       // update that task is completed
       fetch('https://jsonplaceholder.typicode.com/todos/1', {
               method: 'PATCH',
               body: JSON.stringify({
                   completed: true
               }),
               headers: {
                   "Content-type": "application/json; charset=UTF-8"
               
           })
           .then(response => response.json())
           .then(json => console.log(json))

       /* will return
       
           "userId": 1,
           "id": 1,
           "title": "delectus aut autem",
           "completed": true
       
       */
```

As you can see here, the request is very similar to the PUT request, but the body of the request contains only the property of the resource that needs to be changed. The response is the new version of the resource.


# The DELETE method

The DELETE method is used to delete a resource specified by it’s URI.

### Let’s try it

The DELETE request simply looks like this, for deleting a specific resource:

```javascript
       // DELETE task with id = 1
       fetch('https://jsonplaceholder.typicode.com/todos/1', {
           method: 'DELETE'
       })

       // empty response: {}
```

Download the full index.html file from this repo.
