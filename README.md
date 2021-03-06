```
██████╗███████╗██████╗ ██████╗ ███████╗██████╗ ██╗   ██╗███████╗
██╔════╝██╔════╝██╔══██╗██╔══██╗██╔════╝██╔══██╗██║   ██║██╔════╝
██║     █████╗  ██████╔╝██████╔╝█████╗  ██████╔╝██║   ██║███████╗
██║     ██╔══╝  ██╔══██╗██╔══██╗██╔══╝  ██╔══██╗██║   ██║╚════██║
╚██████╗███████╗██║  ██║██████╔╝███████╗██║  ██║╚██████╔╝███████║
╚═════╝╚══════╝╚═╝  ╚═╝╚═════╝ ╚══════╝╚═╝  ╚═╝ ╚═════╝ ╚══════╝

                            /\_/\____,
                  ,___/\_/\ \  ~     /
                  \     ~  \ )   XXX
                    XXX     /    /\_/\___,
                       \o-o/-o-o/   ~    /
                        ) /     \    XXX
                       _|    / \ \_/
                    ,-/   _  \_/   \
                   / (   /____,__|  )
                  (  |_ (    )  \) _|
                 _/ _)   \   \__/   (_
                (,-(,(,(,/      \,),),)

```
##About
Cerberus is a demonstration of a completely stateless and RESTful token-based authorization system using JSON Web Tokens (JWT) and Spring Security.

##Why?
For an API to be truly RESTful, no application state can be stored on the server itself. One particular challenge in implementing this is ensuring that your API is secure. Cerberus is the answer to this problem; access to the endpoints in the API requires a JSON Web Token to be present in the request header. This token is obtained by successfully performing an authentication request with the API, and afterwards this token will grant access to the API based on the authorities granted to the specified user.

##Requirements
Cerberus requires Maven and Java 1.7 or greater.

##Usage
To use start Cerberus, run in the terminal `mvn spring-boot:run`. Cerberus will now be running at `http://localhost:8080/api/`

There are two built-in user accounts to demonstrate the differing levels of access to the endpoints in the API:
```
User - user:password
Admin - admin:admin
```

Cerberus also has two endpoints. The first is the authentication endpoint, which is unrestricted. The second is a protected endpoint which only admin users may access (provided the correct JWT token is present in the request header):
```
/api/auth
/api/protected
```

To authenticate with Cerberus, you can curl a POST request with the following credentials to receive a JWT token:
```
curl -i -H "Content-Type: application/json" -X POST -d '{"username":"admin","password":"admin"}' http://localhost:8080/api/auth
```

The response should look like this:
```
{
  "token" : "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiJ9.bKweskM-3QqOY8ScxhC9AcREOCG2UDY0Ylezdv1h81ALFg_v0QYBgxwfUjtf_Ns7RqAQIh_kFg1ZkeFV-szRUg"
}
```

You can now insert this token into your request header for GET access to `/api/protected`:
```
curl -i -H "Content-Type: application/json" -H "X-Auth-Token: eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiJ9.bKweskM-3QqOY8ScxhC9cREOCG2UDY0Ylezdv1h81ALFg_v0QYBgxwfUjtf_Ns7RqAQIh_kFg1ZkeFV-szRUg" -X GET http://localhost:8080/api/protected
```

You should get an HTTP 200 and the response `:O`

##Testing
To run Cerberus's unit tests, run in the terminal `mvn clean package`.
