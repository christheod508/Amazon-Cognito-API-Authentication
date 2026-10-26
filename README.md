# Amazon-Cognito-API-Authentication
Academic project for the Computer and Network Security course at CEID, University of Patras, demonstrating identity management and secure API access with Amazon Cognito and API Gateway, configured through the AWS Console.

## Overview

The example uses an Amazon Cognito User Pool to authenticate users and
issue tokens. A protected API Gateway endpoint validates the token before
allowing access to the backend.

## Technologies

- Amazon Cognito
- Amazon API Gateway
- JWT
- Python and Flask

## Example

- Request without a valid token: `401 Unauthorized`
- Request with a valid token: `200 OK`

## Project materials

The repository includes the Python configuration and project documentation.
