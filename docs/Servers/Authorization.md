# Authorization Server

### Clients

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 2       | When registering a client, the client developer SHALL:<br/>1) specify the client type as described in Section 2.1<br/>2) provide its client redirection URIs as described in Section 3.1.2<br />3) include any other information required by the authorization server (e.g., application name, website, description, logo image, the acceptance of legal terms). |
| 6749 | 2.1     | The authorization server SHOULD NOT make assumptions about the client type |
| 6749 | 2.1     | If the authorization server does not support clients with different security contexts or does not provide guidance with regard to their registration the client SHOULD register each component as a separate client. |
| 6749 | 2.2     | The client identifier is not a secret; it is exposed to the resource owner and MUST NOT be used alone for client authentication. |
| 6749 | 2.2     | The authorization server SHOULD document the size of any identifier it issues. |

#### Client Authentication

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 2.3     | The authorization server MAY accept any form of client authentication meeting its security requirements. |
| 6749 | 2.3     | The authorization server MAY establish a client authentication method with public clients |
| 6749 | 2.3     | The authorization server MUST NOT rely on public client authentication for the purpose of identifying the client |
| 6749 | 2.3     | The client MUST NOT use more than one authentication method in each request. |

#### Client Password

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 2.3.1   | Clients in possession of a client password MAY use the HTTP Basic authentication scheme as defined in RFC2617 to authentication with the authorization server |
| 6749 | 2.3.1   | The authorization server MUST support the HTTP Basic authentication scheme for authenticating clients that were issues a client password |
| 6749 | 2.3.1   | The authorization server MAY support including client credentials in the request-body using the following parameters: <br />client_id => required<br />client_secret => required (The client MAY omit the parameter if the client secret is an empty string) |
| 6749 | 2.3.1   | Including the client credentials in the request-body using the two parameters is NOT RECOMENDED |
| 6749 | 2.3.1   | Including the client credentials in the request-body using the two parameters SHOULD be limited to clients unable to directly utilize the HTTP Basic authentication scheme |
| 6749 | 2.3.1   | The parameters can only be transmitted in the request-body and MUST NOT be included in the request URI. |
| 6749 | 2.3.1   | The authorization server MUST require the use of TLS as described in Section 1.6 when sending requests using password authentication |
| 6749 | 2.3.1   | Since this client authentication method involves a password, the authorization server MUST protect any endpoint utilizing it against brute force attacks. |

#### Other Authentication Methods

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 2.3.2   | The authorization server MAY support any suitable HTTP authentication scheme matching its security requirements |
| 6749 | 2.3.2   | When using other authentication methods, the authorization server MUST define a mapping between the client identifier (registration record) and authentication scheme. |
|      |         |                                                              |

### Protocol Endpoints

**<u>Two authorization server endpoints:</u>**

**Authorization endpoint** - used by the client to obtain authorization from the resource owner via user-agent redirection

**Token endpoint** - used by the client to exchange an authorization grant for an access token, typically with client authentication

**<u>One client endpoint:</u>**

**Redirection endpoint** - used by the authorization server to return responses containing authorization credentials to the client via the resource owner user-agent

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 3       | Extension grant types MAY define additional endpoints as needed. |

#### Authorization Endpoint

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 3.1     | The authorization server MUST first verify the identity of the resource owner |
| 6749 | 3.1     | The endpoint URI MAY include an "application/x-www-form-urlencoded" formatted query component |
| 6749 | 3.1     | The query component MUST be retained when adding additional query parameters |
| 6749 | 3.1     | The endpoint URI MUST NOT include a fragment component.      |
| 6749 | 3.1     | The authorization server MUST require the use of TLS as described in Section 1.6 when sending requests to the authorization endpoint |
| 6749 | 3.1     | The authorization server MUST support the use of the HTTP "GET" method [RFC2616] for the authorization endpoint |
| 6749 | 3.1     | The authorization server MAY support the use of the "POST" method for the authorization endpoint |
| 6749 | 3.1     | Parameters send without a value MUST be treated as if they were omitted from the request. |
| 6749 | 3.1     | The authorization server MUST ignore unrecognized request parameters |
| 6749 | 3.1     | Request and response parameters MUST NOT be included more than once. |

##### Authorization Endpoint Response

The client informs the authorization server of the desired grant type using the following parameter: response_type

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 3.1.1   | The value of response_type MUST be one of "code" for requesting an authorization code as described by Section 4.1.1, "token" for requesting an access token (implicit grant) as described by Section 4.2.1 or a registered extension value as described by Section 8.4 |
| 6749 | 3.1.1   | Extension responses MAY contain a space-delimited (%x20) list of values, where the order of values does not matter |
| 6749 | 3.1.1   | If an authorization request is missing the "response_type" parameter, or the response type is not understood, the authorization server MUST return an error response as described in Section 4.1.2.1. |

#### Redirection Endpoint

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 3.1.2   | The redirection endpoint URI MUST be an absolute URI as defined by [RFC3986] Section 4.3 |
| 6749 | 3.1.2   | The endpoint URI MAY include an "application/x-www-form-urlencoded" formatted query component |
| 6749 | 3.1.2   | The query component MUST be retained when adding additional query parameters |
| 6749 | 3.1.2   | The endpoint URI MUST NOT include a fragment component       |
| 6749 | 3.1.2.1 | The redirection endpoint SHOULD require the use of TLS as described in Section 1.6 when the requested response type is "code" or"token", or when the redirection request will result in the transmission of sensitive credentials over an open network |
| 6749 | 3.1.2.1 | IF TLS is not available, the authorization server SHOULD warn the resource owner about the insecure endpoint prior to redirection |
| 6749 | 3.1.2.2 | The authorization server MUST require the following clients to register their redirection endpoints: <br />Public clients<br />Confidential clients utilizing the implicit grant type |
| 6749 | 3.1.2.2 | The authorization server SHOULD require all clients to register their redirection endpoint prior to utilizing the authorization endpoint |
| 6749 | 3.1.2.2 | The authorization server SHOULD require the client to provide the complete redirection URI |
| 6749 | 3.1.2.2 | The client MAY use the "state" request parameter to achieve per-request customization |
| 6749 | 3.1.2.2 | If requiring the registration of the complete redirection URI is not possible, the authorization server SHOULD require the registration of the URI scheme, authority, and path (allowing the client to dynamically vary only the query component of the redirection URI when requesting authorization) |
| 6749 | 3.1.2.2 | The authorization server MAY allow the client to register multiple redirection endpoints. |
| 6749 | 3.1.2.3 | If multiple redirection URIs have been registered, if only part of the redirection URI has been registered, or if no redirection URI has been registered, the client MUST include a redirection URI with the authorization request using the "redirect_uri" request parameter. |
| 6749 | 3.1.2.3 | When a redirection URI is included in an authorization request, the authorization server MUST compare and match the value received against at least one of the registered redirection URIs (or URI components) as defined in [RFC3986] Section 6, if any redirection URIs were registered. |
| 6749 | 3.1.2.3 | If the client registration included the full redirection URI, the authorization server MUST compare the two URIs using simple string comparison as defined in [RFC3986] Section 6.2.1 |
| 6749 | 3.1.2.4 | If an authorization request fails validation due to a missing, invalid, or mismatching redirection URI, the authorization server SHOULD inform the resource owner of the error |
| 6749 | 3.1.2.4 | If an authorization request fails validation due to a missing, invalid, or mismatching redirection URI, the authorization server MUST NOT automatically redirect the user-agent to the invalid redirection URI. |
| 6749 | 3.1.2.5 | The client SHOULD NOT include any third-party scripts in the redirection endpoint response. |
| 6749 | 3.1.2.5 | The client SHOULD extract the credentials from the URI and redirect the user-agent again to another endpoint without exposing the credentials |
| 6749 | 3.1.2.5 | If third-party scripts are included, the client MUST ensure that its own scripts (used to extract and remove the credentials from the URI) will execute first. |

#### Token Endpoint

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 3.2     | The endpoint URI MAY include an "application/x-www-form-urlencoded" formatted query component |
| 6749 | 3.2     | The query component MUST be retained when adding additional query parameters |
| 6749 | 3.2     | The endpoint URI MUST NOT include a fragment component.      |
| 6749 | 3.2     | The authorization server MUST require the use of TLS as described in Section 1.6 when sending requests to the token endpoint. |
| 6749 | 3.2     | The client MUST use the HTTP "POST" method when making access token requests. |
| 6749 | 3.2     | Parameters send without a value MUST be treated as if they were omitted from the request |
| 6749 | 3.2     | The authorization server MUST ignore unrecognized request parameters |
| 6749 | 3.2     | Request and response parameters MUST NOT be included more than once. |
| 6749 | 3.2.1   | Confidential clients or other clients issued client credentials MUST authenticate with the authorization server as described in Section 2.3 when making requests to the token endpoint |
| 6749 | 3.2.1   | A client MAY use the "client_id" request parameter to identify itself when sending requests to the token endpoint. |
| 6749 | 3.2.1   | In the "authorization_code" "grant_type" request to the token endpoint, and unauthenticated client MUST send its "client_id" to prevent itself from inadvertently accepting a code intended for a client with a different "client_id" |
| 6749 | 3.3     | The authorization server MAY fully or partially ignore the scope requested by the client, based on the authorization server policy or the resource owner's instructions. |
| 6749 | 3.3     | If the issued access token scope is different from the one requested by the client, the authorization server MUST include the "scope" response parameter to inform the client of the actual scope granted. |
| 6749 | 3.3     | If the client omits the scope parameter when requesting authorization, the authorization server MUST either process the request using a pre-defined default value or fail the request indicating an invalid scope. |
| 6749 | 3.3     | The authorization server SHOULD document its scope requirements and default value (if defined) |

### Access Token Response Types

#### Successful Response

**<u>Parameters:</u>**

- **access_token** => required
- **token_type** => required
- **expires_in** => recommended, if omitted, the authorization server SHOULD provide the expiration time via other means or document the default value
- **refresh_token** => optional
- **scope** => optional if identical to the scope requested by the client; otherwise required

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 5.1     | The authorization server MUST include the HTTP "Cache-Control" response header field [RFC2616] with a value of "no-store" in any response containing tokens, credentials, or other sensitive information, as well as the "Pragma" response header field [RFC2616] with a value of "no-cache" |
| 6749 | 5.1     | The client MUST ignore unrecognized value names in the response |
| 6749 | 5.1     | The authorization server SHOULD document the size of any value it issues. |

#### Error Response

**<u>Response Parameters:</u>**

- **error** => required (invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope)
- **error_description** => optional
- **error_uri** => optional

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 5.2     | error[invalid_client]<br />The authorization server MAY return an HTTP 401 (Unauthorized) status code to indicate which HTTP authentication schemes are supported |
| 6749 | 5.2     | error[invalid_client]<br />If the client attempted to authenticate via the "Authorization" request header field, the authorization server MUST response with an HTTP 401 (Unauthorized) status code and include the "WWW-Authenticate" response header field matching the authentication scheme used by the client. |
| 6749 | 5.2     | Values for the "error" parameter MUST NOT include characters outside the set %x20-21 / %x23-5B / %x5D-7E |
| 6749 | 5.2     | Values for the "error_description" parameter MUST NOT include characters outside the set %x20-21 / %x23-5B / %x5D-7E |
| 6749 | 5.2     | Values for the "error_uri" parameter MUST conform to the URI-reference syntax |
| 6749 | 5.2     | Values for the "error_uri" parameter MUST NOT include characters outside the set %x21 / %x23-5B / %x5D-7E |

### Refreshing an Access Token

Request Parameters:

- grant_type => required MUST be set to "refresh_token"
- refresh_token => required
- scope => optional

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 6       | The requested scope MUST NOT include any scope not originally granted by the resource owner, and if omitted is treated as equal to the scope originally granted by the resource owner |
| 6749 | 6       | If the client type is confidential or the client was issued client credentials (or assigned other authentication requirements), the client MUST authenticate with the authorization server as described in section 3.2.1 |
| 6749 | 6       | The authorization server MUST:<br />require client authentication for confidential clients or for any client that was issued client credentials (or with other authentication requirements)<br />authenticate the client if client authentication is included and ensure that the refresh token was issued to the authenticated client<br />validate the refresh token |
| 6749 | 6       | The authorization server MAY issue a new refresh token       |
| 6749 | 6       | If issued a new refresh token, the client MUST discard the old refresh token and replace it with the new refresh token |
| 6749 | 6       | The authorization server MAY revoke the old refresh token after issuing a new refresh token to the client |
| 6749 | 6       | If a new refresh token is issued, the refresh token scope MUST be identical to that of the refresh token included by the client in the request |

