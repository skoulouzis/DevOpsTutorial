# DevOpsTutorial
Define a simple REST API with OpenAPI and Swagger, write REST API tests using Postman, develop the application logic, dockerize it and finally perform continuous integration (CI) 

## Write the API definitions
To get an understanding of Swagger and OpenAPI you may follow this tutorial till part 5: https://apihandyman.io/writing-openapi-swagger-specification-tutorial-part-1-introduction/. 

### Hands On
Open the swagger editor : http://editor.swagger.io/# .  You should see the ‘Swagger Petstore’ example. 

### Set up example code
<img src="/images/swagger1.png" alt="swagger"
	title="swagger" width="550"/>
	
The existing code is quite complex for a first hands-on. Clear the code:


<img src="/images/swagger2.png" alt="swagger"
	title="swagger" width="550"/>
	
	
<img src="/images/swagger3.png" alt="swagger"
	title="swagger" width="550"/>

Paste the following code into the editor:
[swagger.yaml](swagger/swagger_1.yaml)

You will notice that the editor throws two errors: 
```
Errors 
Semantic error at paths./student.post.parameters.0.schema.$ref
$refs must reference a valid location in the document
Jump to line 27
Semantic error at paths./pet/{student_id}.get.responses.200.schema.$ref
$refs must reference a valid location in the document
Jump to line 52
```
<img src="/images/swagger4.png" alt="swagger"
	title="swagger" width="550"/>

Efectivly what is said here is that the "#/definitions/Student" is not defined. 
You can find more about '$ref' here: https://swagger.io/docs/specification/using-ref/

### Define Objects
Scorll down to the bottom of the page and create a new node called 'definitions' and a node 'Student' under that. The code should look like this:
```YAML
definitions:
  Student:
    type: "object"
    properties:
```

#### Exersise 
Define the Student's object properties. The properties to set are:


| Property Name | Type          		|
| ------------- |:-------------:		| 
| student_id    | integer (int64 format)	| 
| first_name    | string			|  
| last_name     | string			| 
| grades	| map				| 

You can find details about data models here: https://swagger.io/docs/specification/data-models/

The definition sould looke like this: [swagger.yaml](swagger/swagger_2.yaml)

### Add Delete method
The API definition at the moment only has 'GET' and 'POST' methdos. We will add a 'DELETE' method 
Before the 'definitions' node add the following:
```YAML
    delete:
      description: ""
      operationId: "deleteStudent"
      produces:
      - "application/xml"
      - "application/json"   
      responses:
        200:
          description: "successful operation"
          schema:
            $ref: "#/definitions/Student"
        400:
          description: "Invalid ID supplied"
        404:
          description: "student not found"  
```
You will notice that the editor is throwing an error:
```
Errors
Hide
 
Semantic error at paths./pet/{student_id}
Declared path parameter "student_id" needs to be defined within every operation in the path (missing in "delete"), or moved to the path-level parameters object
Jump to line 31
```
#### Exersise 
Fix the 'DELETE' method to remove this error

After you fix the error the definition sould looke like this: [swagger.yaml](swagger/swagger_3.yaml)


### Add Query Parametres to Grt
We will update the get method to include query parametres
In the 'parameters' node add the following:
```YAML
      - name: "subject"
        in: "query"
        description: "The subject name"
        required: false
        type: "string"  
```
The definition sould looke like this: [swagger.yaml](swagger/swagger_4.yaml)

### Generate the Server Code 
### Python-flask 

In the swagger editor go to 'Generate Server' and select 'python-flask'


<img src="/images/swagger5.png" alt="swagger"
	title="swagger" width="550"/>
	
Update the requirements files to look like this:
 [requirements.txt](https://raw.githubusercontent.com/skoulouzis/DevOpsTutorial/req/python-flask-server-generated/python-flask-server/requirements.txt)
 
[test-requirements.txt](https://raw.githubusercontent.com/skoulouzis/DevOpsTutorial/req/python-flask-server-generated/python-flask-server/test-requirements.txt)
  
