---
id: restapi
title: REST API
---

ToolJet can establish connections with any available REST API endpoint, allowing you to create queries and interact with external data sources seamlessly.

<div style={{paddingTop:'24px'}}>

## Connection

To establish a connection with the REST API data source, you can either click on the **+ Add new Data source** button located on the query panel or navigate to the **[Data Sources](/docs/data-sources/overview)** page through the ToolJet dashboard.

<img className="screenshot-full" src="/img/datasource-reference/rest-api/restconnect.gif" alt="ToolJet - Data source - REST API" style={{marginBottom:'15px'}} />

ToolJet requires the following to connect to a REST API data source:

- **Base URL**: REST API endpoint URL
- **Headers**: Key-value pairs to include as headers with REST API requests
- **Authentication Type**: The method of authentication to use with REST API requests. Supported Types: None, Basic, Bearer, and OAuth 2.0
  - **None**: No authentication required
  - **Basic**: Requires Username and Password
  - **Bearer**: Requires a token, typically a JSON Web Token (JWT), to grant access
  - **OAuth 2.0**: Supports both Authorization Code and Client Credentials grant types. Required parameters vary based on the selected grant type and service provider.
    - Access token URL
    - Access token URL custom headers
    - Client ID
    - Client secret
    - Scopes
    - Custom query parameters
    - Authorization URL
    - Custom authentication parameters
    - Client authentication method
- **SSL Certificate**: SSL certificate to use with REST API requests. Supported Types:
  - **None**: No SSL certificate verification.
  - **CA Certificate**: Requires a CA certificate to verify the server certificate.
  - **Client Certificate**: Requires a client certificate and private key to authenticate with the server.
- **Cookies**: Key-value pairs to include as cookies with REST API requests. These cookies will be sent with every query created using this data source instance.

<img className="screenshot-full" src="/img/datasource-reference/rest-api/oauth-v2.png" alt="ToolJet - Data source - REST API" />

</div>

<div style={{paddingTop:'24px'}}>

## Retry on Network Errors

ToolJet provides an option to automatically retry REST API requests in case of certain network errors or specific HTTP status codes. By default, this feature is enabled and will retry the request up to 3 times in case of failure. This feature can be toggled on or off at both the data source level and the individual query level. When enabled, retries will occur for the following scenarios:

1. Specific HTTP status codes: 408, 413, 429, 500, 502, 503, 504, 521, 522, 524.
2. Network errors:
   - **ETIMEDOUT**: One of the timeout limits was reached.
   - **ECONNRESET**: Connection was forcibly closed by a peer.
   - **EADDRINUSE**: Could not bind to any free port.
   - **ECONNREFUSED**: Connection was refused by the server.
   - **EPIPE**: The remote side of the stream being written has been closed.
   - **ENOTFOUND**: Couldn't resolve the hostname to an IP address.
   - **ENETUNREACH**: No internet connection.
   - **EAI_AGAIN**: DNS lookup timed out.

You can configure this feature at two levels:

1. **Data Source Level**: In the REST API data source configuration, you'll find a toggle for **Retry on network errors** This sets the default behavior for all queries using this data source.

<img className="screenshot-full" src="/img/datasource-reference/rest-api/rest-api-data-source.png" alt="ToolJet - Data source - REST API" />

2. **Query Level**: In the query builder for each REST API query, you'll find a similar toggle for for **Retry on network errors** under the **Settings** tab. This sets the behavior for that specific query.

<img className="screenshot-full" src="/img/datasource-reference/rest-api/query-builder-retry.png" alt="ToolJet - Data source - REST API" />

:::info
If the data source-level configuration is enabled but a specific query has it disabled, the query-level setting takes precedence.
:::

</div>

<div style={{paddingTop:'24px'}}>

## Querying REST API

Once you have connected to the REST API data source, follow these steps to write queries and interact with a REST API in the ToolJet application:

1. Click on **+ Add** button of the query manager at the bottom panel of the editor.
2. Select **REST API** from the Data Source section.
3. Enter the required query parameters.
4. Click **Preview** to view the data returned from the query or click **Run** to execute the query.

:::tip
Query results can be transformed using the **[Transformations](/docs/how-to/transformations)** feature.
:::

ToolJet supports the following REST HTTP methods 
- **GET**
- **POST**
- **PUT**
- **PATCH**
- **DELETE**

<img className="screenshot-full" src="/img/datasource-reference/rest-api/restquery.png" alt="ToolJet - Data source - REST API" />

</div>

<div style={{paddingTop:'24px'}}>

## Additional header

Whenever a request is made to the REST API, a **tj-x-forwarded-for** header is added to the request, the value of the header will be the IP address of the user who is logged in to the ToolJet application. This header can be used to identify the user who is making the request to the REST API.

<img className="screenshot-full" src="/img/datasource-reference/rest-api/header.png" alt="ToolJet - Data source - REST API"/>

</div>

<div style={{paddingTop:'24px'}}>

## Cookies

In addition to the data source level cookies, you can add query-specific cookies in the Query builder. These cookies will be sent only with the specific query created using this data source instance.

To add cookies:

1. In the Query builder, navigate to the **Setup** tab.
2. Find the **Cookies** section.
3. Add your cookies as key-value pairs.

You can use both static values and dynamic values for cookie values.

<div style={{textAlign: 'center'}}>
<img className="screenshot-full" src="/img/datasource-reference/rest-api/query-cookies.png" alt="ToolJet - Query Builder - REST API Cookies" />
</div>

 :::info
Query-specific cookies will override data source level cookies with the same name for that particular query.
:::

</div>

<div style={{paddingTop:'24px'}}>

## Request types

The plugin will send a **JSON** formatted body by default. If you want to send a different type of body, you can enter the appropriate headers in the **Headers** section.

For example, to send a **multipart/form-data** body, you can add the following header:

```javascript
  Content-Type: multipart/form-data;
```

<img className="screenshot-full" src="/img/datasource-reference/rest-api/form-headers.png" alt="ToolJet - Data source - REST API" />

<img className="screenshot-full" src="/img/datasource-reference/rest-api/form-body.png" alt="ToolJet - Data source - REST API" />

</div>

<div style={{paddingTop:'24px'}}>

## Response types

REST APIs can return data in a variety of formats, including **JSON** and **Base64**. JSON is a common format used for data exchange in REST APIs, while Base64 is often used for encoding binary data, such as images or video, within a JSON response.
When the response **content-type** is **image**, the response will be a **base64** string.

<details>
<summary>**Example JSON response**</summary>

```json
[
  {
    "id": 1,
    "title": "Fjallraven - Foldsack No. 1 Backpack, Fits 15 Laptops",
    "price": 109.95,
    "description": "Your perfect pack for everyday use and walks in the forest. Stash your laptop (up to 15 inches) in the padded sleeve, your everyday",
    "category": "men's clothing",
    "image": "https://fakestoreapi.com/img/81fPKd-2AYL._AC_SL1500_.jpg",
    "rating": {
      "rate": 3.9,
      "count": 120
    }
  },
  {
    "id": 2,
    "title": "Mens Casual Premium Slim Fit T-Shirts ",
    "price": 22.3,
    "description": "Slim-fitting style, contrast raglan long sleeve, three-button henley placket, light weight & soft fabric for breathable and comfortable wearing. And Solid stitched shirts with round neck made for durability and a great fit for casual fashion wear and diehard baseball fans. The Henley style round neckline includes a three-button placket.",
    "category": "men's clothing",
    "image": "https://fakestoreapi.com/img/71-3HjGNDUL._AC_SY879._SX._UX._SY._UY_.jpg",
    "rating": {
      "rate": 4.1,
      "count": 259
    }
  },
  {
    "id": 3,
    "title": "Mens Cotton Jacket",
    "price": 55.99,
    "description": "great outerwear jackets for Spring/Autumn/Winter, suitable for many occasions, such as working, hiking, camping, mountain/rock climbing, cycling, traveling or other outdoors. Good gift choice for you or your family member. A warm hearted love to Father, husband or son in this thanksgiving or Christmas Day.",
    "category": "men's clothing",
    "image": "https://fakestoreapi.com/img/71li-ujtlUL._AC_UX679_.jpg",
    "rating": {
      "rate": 4.7,
      "count": 500
    }
  },
  {
    "id": 4,
    "title": "Mens Casual Slim Fit",
    "price": 15.99,
    "description": "The color could be slightly different between on the screen and in practice. / Please note that body builds vary by person, therefore, detailed size information should be reviewed below on the product description.",
    "category": "men's clothing",
    "image": "https://fakestoreapi.com/img/71YXzeOuslL._AC_UY879_.jpg",
    "rating": {
      "rate": 2.1,
      "count": 430
    }
  }
]
```

The JSON response can be easily loaded on the components like **table** and **listview** using `{{queries.<queryname>.data}}`

</details>

You can also use JS methods like **map** to load data on components like **dropdown** using **`{{queries.restapi1.data.map(i => i.title)}}`**

<img className="screenshot-full" src="/img/datasource-reference/rest-api/map.png" alt="ToolJet - Data source - REST API" style={{marginBottom:'15px'}} />

<details>
<summary>**Example base64 response**</summary>
```base64
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
```
</details>

Read the guide on **[loading base64 data](/docs/how-to/loading-image-pdf-from-db)**

<img className="screenshot-full" src="/img/datasource-reference/rest-api/base64.png" alt="ToolJet - Data source - REST API" />

</div>

<div style={{paddingTop:'24px'}}>

## Metadata

Metadata is additional information about the data returned by the REST API. This information includes the request URL, method, headers, and response status code, headers, and body. The metadata can be accessed within queries and components using the `{{queries.<queryname>.metadata}}` syntax.

:::info
While accessing the properties of the metadata object, which contains a hyphen, you can use the bracket notation. For example, to access the `user-agent` property, you can use `{{queries.restapi1.metadata.request.headers["user-agent"]}}` or `{{queries.restapi1.metadata.request.headers."user-agent"}}`.
:::

<details>
<summary>**Example Metadata**</summary>

```json
{
  "request": {
    "url": "https://dummyjson.com/users",
    "method": "GET",
    "headers": {
      "user-agent": "got (https://github.com/sindresorhus/got)",
      "tj-x-forwarded-for": "103.171.99.41",
      "accept-encoding": "gzip, deflate, br"
    },
    "params": {}
  },
  "response": {
    "statusCode": 200,
    "headers": {
      "server": "[REDACTED]",
      "report-to": "{"group":"heroku-nel","max_age":3600,"endpoints":[{"url":"https://nel.heroku.com/reports?ts=1726207652&sid=e11707d5-02a7-43ef-b45e-2cf4d2036f7d&s=1ICCahr5yl4s1cOLwZ5JI7Le2a5Hp57L8DugEP6oEZQ%3D"}]}",
      "reporting-endpoints": "heroku-nel=https://nel.heroku.com/reports?ts=1726207652&sid=e11707d5-02a7-43ef-b45e-2cf4d2036f7d&s=1ICCahr5yl4s1cOLwZ5JI7Le2a5Hp57L8DugEP6oEZQ%3D",
      "nel": "{"report_to":"heroku-nel","max_age":3600,"success_fraction":0.005,"failure_fraction":0.05,"response_headers":["Via"]}",
      "connection": "close",
      "access-control-allow-origin": "*",
      "x-dns-prefetch-control": "off",
      "x-frame-options": "SAMEORIGIN",
      "strict-transport-security": "max-age=15552000; includeSubDomains",
      "x-download-options": "noopen",
      "x-content-type-options": "nosniff",
      "x-xss-protection": "1; mode=block",
      "x-ratelimit-limit": "100",
      "x-ratelimit-remaining": "99",
      "date": "Fri, 13 Sep 2024 06:07:32 GMT",
      "x-ratelimit-reset": "1726207656",
      "content-type": "application/json; charset=utf-8",
      "etag": "W/"7d39-+rQ7kyHBCLIn9tjTeKVf4oegWkQ"",
      "vary": "Accept-Encoding",
      "content-encoding": "gzip",
      "transfer-encoding": "chunked",
      "via": "1.1 vegur"
    }
  }
}
```
</details>


</div>

<div style={{paddingTop:'24px'}}>

## OAuth 2.0 Method for Authenticating REST APIs

ToolJet’s REST API data source supports OAuth 2.0 as the authentication type. Supported OAuth 2.0 grant types are Authorization Code and Client Credentials.

- **Authorization Code Grant Type**: This grant type is used by confidential and public clients to exchange an authorization code for an access token.
- **Client Credentials Grant Type**: This grant type is used by clients to obtain an access token outside of the context of a user.

### Setting up Google Cloud Platform

:::info
Before setting up the REST API data source in ToolJet, we need to configure the **Google Cloud Platform** to gather the API keys required for the authorization access.
:::

Google Cloud Platform provides access to more than 350 APIs and Services that can allow us to access data from our Google account and its services. Let's create an OAuth application that can be given permission to use our Google profile data such as Name and Profile picture.

1. Sign in to your [Google Cloud](https://cloud.google.com/) account, and from the console create a New Project.
2. Navigate to the **APIs and Services**, and then open the **OAuth consent screen** section from the left sidebar.
3. Enter the Application details and select the appropriate scopes for your application. We will select the profile and the email scopes. 
4. Once you have created the OAuth consent screen, Create new credentials for the **OAuth client ID** from the **Credentials** section in the left sidebar.
5. Select the application type, enter the application name, and then add the following URIs under Authorized Redirect URIs:
    1. `https://app.tooljet.com/oauth2/authorize` (if you’re using ToolJet cloud)
    2. `http://localhost:8082/oauth2/authorize` (if you’re using ToolJet locally)
6. Now save and then you’ll get the **Client ID and Client secret** for your application.

<img class="screenshot-full" src="/img/how-to/oauth2-authorization/gcp.png" alt="ToolJet - How To - REST API authentication using OAuth 2.0" style={{marginBottom:'15px'}}/>

### Configuring ToolJet Application with Google's OAuth 2.0 API

Let's follow the steps to authorize ToolJet to access your Google profile data:

1. Go to the **Data Sources** page from the ToolJet dashboard, select API category on sidebar and choose the **REST API** data source.
2. In the **Base URL** field, enter the base URL `https://www.googleapis.com/oauth2/v1/userinfo`; the base URL specifies the network address of the API service.
3. Select **Authentication** type as *OAuth 2.0*
4. Keep the default values for **Grant Type**, **Add Access Token To**, and **Header Prefix** i.e. *Authorization Code*, *Request Header*, and *Bearer* respectively.
5. Enter **Access Token URL**: `https://oauth2.googleapis.com/token`; this token allows users to verify their identity, and in return, receive a unique access token.
6. Enter the **Client ID** and **Client Secret** that we generated from the [Google Console](http://console.developers.google.com/).
7. In the **Scope** field, enter `https://www.googleapis.com/auth/userinfo.profile`; Scope is a mechanism in OAuth 2.0 to limit an application's access to a user's account. Check the scopes available for [Google OAuth2 API here](https://developers.google.com/identity/protocols/oauth2/scopes#oauth2).
8. Enter **Authorization URL:** `https://accounts.google.com/o/oauth2/v2/auth`; the Authorization URL requests authorization from the user and redirects to retrieve an authorization code from identity server.
9. Create three **Custom Authentication Parameters:**
    1. **response_type**: code ( `code` refers to the Authorization Code)
    2. **client_id**: Client ID
    3. **redirect_url**: `http://localhost:8082/oauth2/authorize` if using ToolJet locally or enter this `https://app.tooljet.com/oauth2/authorize` if using ToolJet Cloud.
10. Keep the default selection for **Client Authentication** and **Save** the data source.

<img class="screenshot-full" src="/img/how-to/oauth2-authorization/restapi-v2.png" alt="ToolJet - How To - REST API authentication using OAuth 2.0" style={{marginBottom:'15px'}}/>

### Authenticating REST API

Let’s create a query to make a `GET` request to the URL, it will pop a new window and ask the user to authenticate against the API.

- Add a new query and select the REST API data source from the dropdown
- In the **Method** dropdown select `GET` and enable the  `Run query on application load?`
- Run the query. 
- A new window will pop for authentication and once auth is successful, you can run the query again to get the user data like Name and Profile Picture.

</div>

<div style={{paddingTop:'24px'}}>

## Bearer Token Method for Authenticating REST APIs

ToolJet’s REST API data source supports Bearer Token as the authentication type. Bearer Token is a security token that is issued by the authentication server to the client. The client then uses the token to access the protected resources hosted by the resource server.

### Configuring REST API Data Source with Bearer Token

1. Go to the **Data Sources** page from the ToolJet dashboard, select **API** category on sidebar and choose the **REST API** data source.
2. In the **Base URL** field, enter the base URL. The base URL specifies the network address of the API service. For example, `http://localhost:3001/api/bearer-auth`
3. Enter the **Headers** if required. Headers are key-value pairs to include as headers with REST API requests.
4. Select **Authentication** type as *Bearer* from the dropdown.
5. Enter the **Token** in the field. The token is a security token that is issued by the authentication server to the client. The client then uses the token to access the protected resources hosted by the resource server.
  
<img className="screenshot-full" src="/img/datasource-reference/rest-api/none.png" alt="ToolJet - Data source - REST API" />

6. Now you have option to select the **SSL Certificate** if required. SSL certificate is used to verify the server certificate. By default, it is set to *None*. You can provide the **CA Certificate** or **Client Certificate** from the dropdown.
    1. **CA Certificate**: Requires a CA certificate to verify the server certificate. Copy the content of `server.crt` file and paste it in the **CA Cert** field. `server.crt` file is the certificate file that is used to verify the server certificate.
  
        <img className="screenshot-full" src="/img/datasource-reference/rest-api/cacert.png" alt="ToolJet - Data source - REST API" />

    2. **Client Certificate**: Requires a client certificate to authenticate with the server. **client.key**, **client.crt**, and **server.crt** files are the certificate files that are used to authenticate with the server. Copy the content of **client.key** file and paste it in the **Client Key** field. Copy the content of **client.crt** file and paste it in the **Client Cert** field. Copy the content of **server.crt** file and paste it in the **CA Cert** field.
  
        <img className="screenshot-full" src="/img/datasource-reference/rest-api/clientcert.png" alt="ToolJet - Data source - REST API" />

7. Once you have configured the REST API data source, click on the **Save** button.

### Authenticating REST API

Create a query to make a `GET` request to the URL, and it will return a success message if the token is valid.

<div style={{textAlign: 'center'}}>

<img className="screenshot-full" src="/img/datasource-reference/rest-api/bearersuccess.png" alt="ToolJet - Data source - REST API" />

</div>

</div>
