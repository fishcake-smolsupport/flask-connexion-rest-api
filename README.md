# flask-connexion-rest-api
In following the RealPython course on the same topic. This is a 3-part course.


## Caveats
### Part 1: REST APIs With Flask + Connexion
- In dependency installation I simply went with:

```Shell
(venv) $ pip install flask
(venv) $ pip install "connexion[swagger-ui]"
```

Due to this, as of Flask 3.0.3, Connexion 3.1.0, additional dependencies were necessary:

```Shell
(venv) $ pip install "connexion[flask]"
(venv) $ pip install "connexion[uvicorn]"
```

In addition, due to the versions of Flask and connexion, x-body-name: "person" goes directly below requestBody instead of content/application/json/schema e.g. instead of:

```YAML
    put:
      tags:
        - People
      operationId: "people.update"
      summary: "Update a person"
      parameters:
        - $ref: "#/components/parameters/lname"
      responses:
        "200":
          description: "Successfully updated person"
      requestBody:
        content:
          application/json:
            schema:
              x-body-name: "person"
              $ref: "#/components/schemas/Person"
```
it was: 

```YAML
    put:
      tags:
        - People
      operationId: "people.update"
      summary: "Update a person"
      parameters:
        - $ref: "#/components/parameters/lname"
      responses:
        "200":
          description: "Successfully updated person"
      requestBody:
        x-body-name: "person"
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/Person"
```

I take no credit for this discovery and would like to thank @dan-0197998 who left this helpful note in the comments, allowing me to finish this tutorial.
This correction has a direct impact and is the deciding factor on whether the POST and PUT requests work or throw a 500 response.