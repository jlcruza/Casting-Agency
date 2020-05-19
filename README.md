# Udacity FSND Capstone Project: Casting Agency

This is my final project as part of the Udacity Fullstack Nanodegree program. It's based on the suggested project Casting Agency, which models a company that is responsible for creating movies and managing and assigning actors to those movies.

## Getting Started
### Project dependencies and local development

To run locally, this project requires the user have Python3, pip3 and postgres installed.

To install all dependencies run the following command:
```
pip3 install -r requirements.txt
```

### hosting instructions

To start the API, run the following command:
```
python3 app.py
```

This API is also hosted on Heroku at the following URL
https://jorge-casting-agency.herokuapp.com/

## Tests
Unittest were created to test all endpoints of the API.
To run the tests, you need to run the following commands:

```
dropdb moviecenter
createdb moviecenter
psql moviecenter < moviecenter.sql
python3 test_api.py
```

**Note:** You wont need to run `dropdb moviecenter` the first time, since the database doesn't exist yet.

## API Reference

### Getting Started
- Base URL
  - If run locally: http://127.0.0.1:8080/ 
  - Hosted at Heroku: https://jorge-casting-agency.herokuapp.com/

- Authentication: API handles three types of Roles, each with its own set of permissions.
  - Casting Assistance: Can read actors and movies.
  - Casting Director: What Casting Assistance can do, plus add or delete actors, and modify the information of actors and movies.
  - Executive Producer: What Casting Director can do, plus add and delete movies.

### Error Handling
All responses of the API, including requests that result in error, will be returned in JSON format. For example:
```
{
    "error": 401,
    "message": "error with payload",
    "success": false
}
```

The API return the following error codes:
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Resource Not Found
- 405: Method Not Allowed
- 422: Unprocessable

### Endpoints

**GET /actors**  
Retrieves all actors from the database. Requieres `read:actors` permission.
- Return a success value, a list of actor objects and the total of actors.
```
{
    "actors": [
        {
            "age": 10,
            "bio": "Random text",
            "gender": "Male",
            "id": 1,
            "name": "Actor 1",
            "picture_link": "https://cdn.pixabay.com/photo/2018/05/01/07/55/boy-3364927_1280.jpg"
        },
        {
            "age": 30,
            "bio": "My bio",
            "gender": "Male",
            "id": 2,
            "name": "Actor 1",
            "picture_link": "https://cdn.pixabay.com/photo/2018/07/06/19/48/charles-chaplin-3521070_1280.jpg"
        },
        {
            "age": 40,
            "bio": "My bio",
            "gender": "Female",
            "id": 3,
            "name": "Actor 2",
            "picture_link": "https://cdn.pixabay.com/photo/2018/07/06/19/48/charles-chaplin-3521070_1280.jpg"
        }
    ],
    "success": true,
    "total": 3
}
```

**GET /movies**  
Retrieves all movies from the database. Requieres `read:movies` permission.
- Return a success value, a list of movie objects and the total of movies.
```
{
    "movies": [
        {
            "id": 1,
            "picture_link": "https://cdn.pixabay.com/photo/2015/01/11/09/19/film-596009_1280.jpg",
            "released": "Wed, 13 May 2020 00:00:00 GMT",
            "synopsis": "Random Text",
            "title": "Movie 1"
        }
    ],
    "success": true,
    "total": 1
}
```

**DELETE /actors/{id}**  
Permantly delete an actor from the database. Requieres `delete:actors` permission.
- Return a success value and the id of the deleted actor.
```
{
    "deleted": 3,
    "success": true
}
```

**DELETE /movies/{id}**  
Permantly delete a movie from the database. Requieres `delete:movies` permission.
- Return a success value and the id of the deleted movie.
```
{
    "deleted": 2,
    "success": true
}
```

**POST /actors**  
Creates an actor in the database. Requieres `create:actors` permission.
- Return a success value and the id of the created actor.
```
{
    "created": 3,
    "success": true
}
```

**POST /movies**  
Creates a movie in the database. Requieres `create:movies` permission.
- Return a success value and the id of the created movie.
```
{
    "created": 2,
    "success": true
}
```

**PATCH /actors/{id}**  
Updates an actor's information in the database. Requieres `patch:actors` permission.
- Return a success value and the modified actor 
```
{
    "actor": {
        "age": 28,
        "bio": "Random text",
        "gender": "Female",
        "id": 1,
        "name": "Mod 1",
        "picture_link": "https://cdn.pixabay.com/photo/2016/11/08/05/15/dancer-1807516_1280.jpg"
    },
    "success": true
}
```

**PATCH /movies/{id}**  
Updates a movie's information in the database. Requieres `patch:movies` permission.
- Return a success value and the modified actor 
```
{
    "movie": {
        "id": 1,
        "picture_link": "sample.com",
        "released": "Sun, 09 Feb 2020 00:00:00 GMT",
        "synopsis": "Now is another actor",
        "title": "Mod 1"
    },
    "success": true
}
```

**POST /cast**  
Link an actor with a movie. Requieres `patch:movies` permission.
- Return a success value and the created cast id.
```
{
    "created": 4,
    "success": true
}
```

**DELETE /cast/{movies_id}/{actors_id}**  
Deletes an Actor from a Movie cast. Requieres `patch:movies` permission.
- Return a success value and the deleted cast id.
```
{
    "deleted": 4,
    "success": true
}
```

**GET /actors/{id}/cast**  
Retrieves movies done by the actor. Requieres `read:movies` permission.
- Return a success value and the list of movie objects the actor has.
```
{
    "movies": [
        {
            "movies_id": 1,
            "picture_link": "sample.com",
            "released": "Sun, 09 Feb 2020 00:00:00 GMT",
            "title": "Mod 1"
        }
    ],
    "success": true
}
```

**GET /movies/{id}/cast**  
Retrieves actors of the movie. Requieres `read:actors` permission.
- Return a success value and the list of actor objects that casted the movie.
```
{
    "actors": [
        {
            "age": 28,
            "bio": "Random text",
            "gender": "Female",
            "id": 1,
            "name": "Mod 1",
            "picture_link": "https://cdn.pixabay.com/photo/2016/11/08/05/15/dancer-1807516_1280.jpg"
        }
    ],
    "success": true
}
```

**GET /movies/{id}/nocast**  
Retrieves actors that are not in the movie. Requieres `read:actors` permission.
- Return a success value and the list of actor objects that didn't casted the movie.
```
{
    "actors": [
        {
            "age": 30,
            "bio": "My bio",
            "gender": "Male",
            "id": 2,
            "name": "Actor 1",
            "picture_link": "https://cdn.pixabay.com/photo/2018/07/06/19/48/charles-chaplin-3521070_1280.jpg"
        }
    ],
    "success": true
}
```