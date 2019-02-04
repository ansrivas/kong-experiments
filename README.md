Using Kong as an API Gateway:
---

## Requirements:

1. docker-engine
2. docker-compose
3. curl

## Usage:

1. Run the services using `docker-compose up` in one terminal.
2. In about 15 seconds all the service should be up and running.
3. Verify that you have 3 services running using `docker ps`.
  - flask web app
  - postgres database
  - kong
4. At this point check if the admin-url of kong is reachable:

    `curl -i http://localhost:8001/`
5. Now register your local flask app with a unique name: `flask-greeter`

    ```
    curl -i -X POST \
      --url http://localhost:8001/services/ \
      --data 'name=flask-greeter' \
      --data 'url=http://0.0.0.0:5000'
    ```
6. Define its unique hostname (flask-greeter.com) and register the route:

  ```
    curl -i -X POST \
    --url http://localhost:8001/services/flask-greeter/routes \
    --data 'hosts[]=flask-greeter.com'
  ```

7. Now test the follow routes to see if they are properly redirected:
    ```
      curl -i -X GET \
        --url http://localhost:8000/ \
        --header 'Host: flask-greeter.com'
    ```

    ```
      curl -i -X GET \
        --url http://localhost:8000/user/MYNAME \
        --header 'Host: flask-greeter.com'
    ```
