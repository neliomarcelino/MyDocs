# OAuth2
## Example Diagram
![[Cognito_OAuth2.0_diagram.png|600]]

1.  Invoke AWS Cognito /oauth2/token endpoint with grant_type as client_credentials. Refer [https://docs.aws.amazon.com/cognito/latest/developerguide/token-endpoint.html](https://docs.aws.amazon.com/cognito/latest/developerguide/token-endpoint.html)
2. If the request is valid, AWS Cognito will return a JWT (JSON Web Token) formatted access_token
3. Pass this token in Authorization header for all API calls
4. API Gateway makes a call to AWS Cognito to validate the access_token
5. AWS Cognito returns token validation response
6. If token is valid, API Gateway will validate the OAuth2 scope in the JWT token and ALLOW or DENY API call. This is entirely handled by API Gateway once configuration is in place
7. Perform the actual API call whether it is a Lambda function or custom web service application
8. Return the results from Lambda function
9. Return result to API Gateway
10. If there are no issues with the Lambda function, API Gateway will return a HTTP 200 with response data to the client application

## Configurations
### Cognito
1. Create `user pool`
2. Define `Domain name`
3. Add an `App client name`
4. Create a `Resource server`
	1. `Identifier` could be the API Gateway endpoint
	2. `Scopes` secures the API, define API's paths for each scope
5. Define on `App client settings > OAuth 2.0 > Client credentials`  and select the `Allowed Custom Scopes`
6. Read how to call API to get the access token [AWS Docs](https://docs.aws.amazon.com/cognito/latest/developerguide/token-endpoint.html)
7. Send request to cognito API `http://[...].auth.[AWS_REGION].amazoncognito.com/oauth2/token:

> [!Request]
	POST
		https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/token&
			Content-Type='application/x-www-form-urlencoded'&
			Authorization=Basic [APP_CLIENT_SECRET]
			grant_type=[authorization_code | refresh_token | client_credentials]&
			client_id=[APP_CLIENT_ID]&
			code=AUTHORIZATION_CODE&
			redirect_uri=com.myclientapp://myclient/redirect
	
> [!Response]
	HTTP/1.1 200 OK
		Content-Type: application/json 
		{
			"access_token":"eyJz9sdfsdfsdfsd[...]",
			"id_token":"dmcxd329ujdmkemkd349r",
			"token_type":"Bearer",
			"expires_in":3600
		}

8. Use bearer `access_token` to authenticate

## Step by step 
---
links:
	- [Part 1 : Securing AWS API Gateway using AWS Cognito OAuth2 scopes | 2022-09-29](https://awskarthik82.medium.com/part-1-securing-aws-api-gateway-using-aws-cognito-oauth2-scopes-410e7fb4a4c0)
	- [Using OAuth 2.0 on API Gateway with custom authorizer](https://aws.amazon.com/blogs/security/use-aws-lambda-authorizers-with-a-third-party-identity-provider-to-secure-amazon-api-gateway-rest-apis/)


#aws/cognito
#aws/cognito/OAuth
#OAuth