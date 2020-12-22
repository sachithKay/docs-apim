# Getting Started - Publisher API v0.15

!!! warning
    - The **Publisher v0.15 REST APIs will be deprecated in the next WSO2 API Manager release**. 
    - **[WSO2 recommends that you use the Publisher v1 REST APIs]({{base_path}}/develop/product-apis/publisher-apis/publisher-v1/publisher-v1/)** as it up to date and has support for the latest WSO2 API Manager 3.0.0 features.

WSO2 API Manager is a complete solution for publishing APIs, creating and managing a developer community, and for scalably routing API traffic. It leverages proven, production-ready, integration, security and governance components from WSO2 Enterprise Service Bus, WSO2 Identity Server, and WSO2 Governance Registry. Moreover, it is powered by API Manager Analytics, thereby making WSO2 API Manager ready for any large-scale deployments right away.

The REST API is implemented based on REST best practices and specifications as a CXF REST web application running on WSO2 API Manager. API development is started with a Swagger specification with a contract-first approach. Please see full [Swagger definition](https://raw.githubusercontent.com/wso2/carbon-apimgt/v6.5.349/components/apimgt/org.wso2.carbon.apimgt.rest.api.publisher/src/main/resources/publisher-api.yaml) which is written using Swagger 2.0. This can be also retrieved from the Web app itself using the URL `https://<host-name[:port]>/api/am/publisher/v0.15/swagger.json`.

The API comes with a pluggable security mechanism. Since API security is implemented as a CXF handler, if you need to plug a custom security mechanism, you can write your own handler and add it to the web service.

Before invoking the API with the access token, obtain the consumer key/secret key pair by calling the dynamic client registration endpoint. You can request an access token with the preferred grant type. An example is shown below (In this example, the payload.json is saved in the same directory where the curl command is executed).

```
curl -k -X POST -H "Authorization: Basic YWRtaW46YWRtaW4=" -H "Content-Type: application/json" -d @payload.json https://localhost:9443/client-registration/v0.15/register
```

Sample request:

```
{
   "callbackUrl":"www.google.lk",
   "clientName":"rest_api_publisher",
   "owner":"admin",
   "grantType":"password refresh_token",
   "saasApp":true
}
```

Sample response:

```
{
   "callBackURL":"www.google.lk",
   "jsonString":"{ \"username\":\"admin\", \"redirect_uris\":\"www.google.lk\", \"client_name\":\"admin_rest_api_publisher\", \"grant_types\":\"authorization_code password refresh_token iwa:ntlm urn:ietf:params:oauth:grant-type:saml2-bearer client_credentialsimplicit\" }",
   "clientName":null,
   "clientId":"HfEl1jJPdg5tbtrxhAwybN05QGoa",
   "clientSecret":"l6c0aoLcWR3fwezHhc7XoGOht5Aa"
}
```

Next, you must use the above client id and clientSecret to obtain the access token. We will be using the password grant type for this, you can use any grant type you desire. You also need to add the proper scope when getting the access token. All possible scopes for Publisher REST API can be viewed in the `securityDefinitions-->OAuth2Security` section and the scope for each resource is given in the `security-->OAuth2Security` section of resource documentation. 
A sample call to generate the access token is shown below.

!!! note 
    The consumer key and consumer secret keys must be Base64 encoded in the format `consumer-key:consumer-secret`

```
curl -k -d "grant_type=password&username=admin&password=admin&scope=apim:api_view" -H "Authorization: Basic SGZFbDFqSlBkZzV0YnRyeGhBd3liTjA1UUdvYTpsNmMwYW9MY1dSM2Z3ZXpIaGM3WG9HT2h0NUFh" https://localhost:8243/token
```

Token response:

```
{
   "scope":"apim:api_view",
   "token_type":"Bearer",
   "expires_in":3600,
   "refresh_token":"33c3be152ebf0030b3fb76f2c1f80bf8",
   "access_token":"292ff0fd256814536baca0926f483c8d"
}
```

Now you have a valid access token, which you can use to invoke an API. Navigate through the API descriptions to find the required API, obtain an access token as described above and invoke the API with the authentication header. If you use a different authentication mechanism, this process may change.

Note: The implementation of WSO2 API Manager is similar to DCR. Since retrieve client application, edit, and delete is only available in DCRM specifications you cannot perform these actions using REST API. However, you can view the created OAuth2 application using the Management Console. Please see [Accessing the Management Console]({{base_path}}/install-and-setup/installation-guide/running-the-product/#accessing-the-management-console) for more details.

Further, to configure the Publisher REST API, do the necessary configurations mentioned in [configuring REST APIs]({{base_path}}/develop/product-apis/configuring-rest-api/).

!!! note
    To get a detailed overview of Publisher REST API, follow [Publisher API v0.15]({{base_path}}/develop/product-apis/publisher-apis/publisher-v0.15/publisher-v0.15/).