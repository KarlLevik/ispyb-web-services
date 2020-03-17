# Future ISPyB Web Services: Development guidelines 

March 2020 

## Introduction 

This document is intended to start discussion on how future web services can be developed as part of the ISPyB collaboration. It sets out software language and design principles for new services. The general agreement across the collaboration is that future web services should be written in Python and stick with a MariaDB database solution; therefore, libraries mentioned below reflect that environment. 

## Service Design 

The intent behind developing new web services for ISPyB are to encourage more collaboration from organisations and take advantage of lessons learned to date developing software for ISPyB. It is expected that some (most?) services are expected to provide a RESTful interface to their capabilities. GraphQL is also a candidate for some more complex services (e.g. beamline activity). 

The Open Web Application Security Project (OWASP) https://owasp.org/ provides good background reading on web application design from a security perspective. There are a number of recommendations and a REST security cheat sheet available here: https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html 

Another useful source of best practice consideration for REST APIs is here: https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design 

…and 

https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md 

These are large documents with many considerations for designing large scale solutions. However, we can start by identifying key areas such as “Consistency Fundamentals”, “Collections”, “JSON standardizations” and “Versioning” (from the link above). 

The aim of this document is to define a number of recommendations that can be agreed by the collaboration. This initial version made available for editing/commenting captures a few topic areas and proposes/raises questions that we should discuss and agree a way forward on.  

The next sections cover some design principles for REST services. However, some points such as naming conventions are equally applicable to GraphQL queries. Each section could end with a recommendation that is agreed by the collaboration (e.g. see below). 

## Recommendations 

### Design documentation 

We should aim to develop consistent and comprehensive documentation for each service. 

As a minimum for REST services an openapi definition should be developed. https://swagger.io/docs/specification/about/ 

For GraphQL the equivalent is the schema language. https://graphql.org/learn/schema/ 

Is it feasible to ensure each service has at least a single process diagram for its operation? The reality is that services might be used across multiple use cases. So the mapping from a user process to a web service may be difficult to keep up to date? 

Recommendation: Use openapi specification to document any REST web service API. 

### URL 

The design for each endpoint should follow consistent practices. We should agree on guidelines for URL design e.g. use plural nouns for resources (e.g. dewars/ not dewar), use of HTTP verbs to express intent (not using get, list, remove in url), and avoid passing sensitive information in URLs (e.g. use tokens in header such as “Authorization: Bearer <token>”). 

How to handle naming conventions? 

    Use of abbreviations to shorten urls?  

    Dc vs datacollections?  

    Prop vs proposal? 

    Visit vs session? 

Recommendation: There are many – which ones should we capture here? 

### Filter Results 

The same endpoint could support queries for users and staff with different levels of permission. 

Each service should describe what permissions are used to filter results. 

What rules should be in place to use query parameters compared with parameters stored within the request payload? 

Recommendation: ? 

### Pagination 

Pagination should be used for endpoints that return large data sets. ISPyB at large facilities can contain many thousands of records so pagination on the server side is necessary to provide a good user experience for users (mobile or otherwise). 

Client agnostic query parameters such as “limit” and “offset” can be used to describe number of records and the index from which to start. 

Recommendation: ? 

### Validation 

Validation should be used to ensure endpoint is used correctly.  

It would be convenient to use a common library for serialization and validation for the web service. 

Recommendation: ? 

### Status Codes 

Consistent use of http status codes (400 for bad request, 404 not found etc.) 

Probably don’t need to use every type available but use should be documented and consistent. 

This should be captured in the API design specification for each service. 

Recommendation: ? 

### Versioning 

New services will likely represent a significant departure from current solutions. Therefore, adding versioning information into the services from the start would be sensible. The Microsoft guidance supports two approaches: 

    Versioning in URL 
    https://api.example.com/v1.0/products/users 

    Versioning in query parameter 
    https://api.example.com/products/users?api-version=1.0 

Versioning can be limited to the major version number (e.g. v1). Note versioning via a custom header (e.g. Custom-Header: api-version=1.0) is not recommended. 

Recommendation: Use major version in URL (v2 to indicate new services?)  

### Authorization 

We still need to establish the overall system design – how will services interact with each other (if at all?) Where does responsibility for authorization (not authentication) reside? In the service or as part of an API gateway? Should permissions be included in a JSON web token? This might be simplest solution as then a service just needs to deal with if the permissions have been set rather than include user queries in its database queries. 

Recommendation: Needs more discussion… 

## Technical choices 

For discussion and agreement – suggest others based on your own experience…Could turn this into a table with suggestions from each site if there is a significant variation in views…? 

IMHO, the document should link to resources describing each choice 

Language: Python 

Code style: Black (opinionated style guide)  

https://black.readthedocs.io/en/stable/ 

Web framework: FlaskRestful, Graphene (GraphQL) 

https://flask-restful.readthedocs.io/en/latest/ 

https://graphene-python.org/ 

Database interface: SQLAlchemy – support for stored procedures? 

https://www.sqlalchemy.org/ 

Validation and serialisation library: Marshmallow 

https://marshmallow.readthedocs.io/en/stable/ 

Documentation: OpenAPI spec (can be generated from python code using apispec) 

Test library: Need to be realistic over unit test coverage (e.g. pytest, flask testing libraries) vs API testing. 

Deployment environment: wsgi service e.g. gunicorn? 

 

## Governance Process (Longer term consideration) 

The governance process will be defined in a separate document, to be approved by the steering committee. It will specify): 

    Who has permission to merge changes onto master or release branches? 

    How many approvals are required to allow merges? 

    Which collaboration members can/will approve changes? 
