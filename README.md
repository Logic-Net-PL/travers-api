# Travers API
## Context
API has been created to make system able to receive delivery orders directly from
third part company software without manually processing each by dedicated application.

## Authorization and Security
Token Based Authorization (Bearer Token)

- Each token can be generated with 3 types of permissions:
    - Read (`GET` endpoints only)
    - Write (`POST`, `PATCH`, `PUT`, `DELETE` endpoints)
    - Read & Write (All available endpoints)
- Tokens are individually generated for project by Travers
- To get more details about API usage feel free to reach out directly

## Documentation
- API documentation is available [here](docs/API.md)
- Postman initial configuration is available [here](postman/Travers API.postman_collection.json)
