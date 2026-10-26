# Identity Management & Secure API Access with Amazon Cognito

Academic project for the **Computer and Network Security** course at the Department of Computer Engineering and Informatics (CEID), University of Patras.

The project demonstrates user authentication with Amazon Cognito and JWT-based access control for an endpoint protected by Amazon API Gateway. AWS services are configured through the AWS Console; a small Flask application provides a local login example.

## Architecture

1. The user opens the local Flask application and selects **Login**.
2. The browser is redirected to the Amazon Cognito Managed Login page.
3. After successful authentication, Cognito redirects to the Flask `/authorize` callback and issues tokens.
4. A client sends an access token as a Bearer token to the protected API Gateway endpoint.
5. API Gateway validates the JWT. The example endpoint uses an HTTP integration with `https://httpbin.org/get`.

## Project files

- `Amazon Cognito/configuration.py` — Flask application and OpenID Connect integration with Cognito.
- `Αναφορά project_1100557.pdf` — project report.
- `παρουσίαση PPT.odp` — project presentation.

## Run the Flask app locally

### Requirements

- Python 3
- An Amazon Cognito User Pool and app client configured for OAuth 2.0 / OpenID Connect
- Flask and Authlib

From the repository root, create and activate a virtual environment, then install the dependencies:

```powershell
cd "Amazon Cognito"
py -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install Flask Authlib
```

In `configuration.py`, configure the Cognito `authority`, app `client_id`, and `client_secret` for your own User Pool and app client. Do not commit real secrets. In the `/login` route, set the callback URL to the local `/authorize` route:

```python
redirect_uri = url_for('authorize', _external=True)
return oauth.oidc.authorize_redirect(redirect_uri)
```

In the Cognito app client settings, add `http://localhost:5000/authorize` as an allowed callback URL, and allow the scopes used by the application (`openid`, `email`, and `phone`). Make sure the local callback URL and the URL sent by the application match exactly.

Start the app:

```powershell
python configuration.py
```

Open [http://localhost:5000](http://localhost:5000). The home page displays **“Welcome! Please Login.”** before authentication. After a successful login, it displays **“Hello, <email>.”** with a logout link. Logging out clears the local session and returns to the home page.

## Protected API check

The project report describes a `GET /protected` API Gateway route. Use your deployed endpoint and a valid Cognito access token:

```bash
curl -i -X GET "https://<api-id>.execute-api.<region>.amazonaws.com/protected" \
  -H "Authorization: Bearer <ACCESS_TOKEN>"
```

Expected results from the project demonstration:

- Without an access token: **HTTP 401 Unauthorized**.
- With a valid access token: **HTTP 200 OK**, with a JSON response from the `httpbin.org` integration.

The API endpoint, Cognito User Pool, app client, callback URLs, and tokens must be replaced with values from your own AWS setup. Never publish access tokens, client secrets, or other credentials.

## Notes

This is an academic demonstration, not a production deployment guide. Run Flask's development server only for local development; review the app's secret, debug, and deployment settings before using it in production. Do not commit the virtual environment (`.venv`) or local credential files.
