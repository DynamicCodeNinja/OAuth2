# Authorization Code

## Authorization Request

**<u>Request Parameters</u>**
**response_type** => required MUST be set to "code"
**client_id** => required
**redirect_uri** => optional
**scope** => optional
**state** => recommended SHOULD be used for preventing cross-site request forgery as described in section 10.12

**<u>Response Parameters</u>**

**code** => required
**state** => required if present in request; must be the exact value as received in the require

**<u>Error Parameters</u>**

**error** => required (*invalid_request*, *unauthorized_client*, *access_denied*, *unsupported_response_type*, *invalid_scope*, *server_error*, *temporarily_unavailable*)
**error_description** => optional
**error_uri** => optional
**state** => required if present in request; must be the exact value as received in the require

------

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 4.1.2   | The authorization code MUST expire shortly after it is issued to mitigate the risk of leaks |
| 6749 | 4.1.2   | A maximum authorization code lifetime of 10 minutes is RECOMENDED |
| 6749 | 4.1.2   | The client MUST NOT use the authorization code more than once |
| 6749 | 4.1.2   | If an authorization code is used more than once, the authorization server MUST deny the request |
| 6749 | 4.1.2   | If an authorization code is used more than once, the authorization server SHOULD revoke (when possible) all tokens previously issued based on that authorization code |
| 6749 | 4.1.2   | The client MUST ignore unrecognized response parameters      |
| 6749 | 4.1.2   | The authorization server SHOULD document the size of any value it issues |
| 6749 | 4.1.2.1 | If the request fails due to a missing, invalid, or mismatching redirection URI, or if the client identifier is missing or invalid, the authorization server SHOULD inform the resource owner of the error |
| 6749 | 4.1.2.1 | If the request fails due to a missing, invalid, or mismatching redirection URI, or if the client identifier is missing or invalid, the authorization server MUST NOT automatically redirect the user-agent to the invalid redirection URI |

## Access Token Request

**<u>Request Parameters:</u>**

**grant_type** => required (*authorization_code*)
**code** => required
**redirect_uri** => required, if included in the authorization request
**client_id** => required, if not authenticating with the authorization server as described in Section 3.2.1

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 4.1.3   | The value for "grant_type" MUST be set to "authorization_code" |
| 6749 | 4.1.3   | The "redirect_uri" parameter is required if it was included int he authorization request as described in section 4.1.1, and their values MUST be identical |
| 6749 | 4.1.3   | If the client type is confidential or the client was issued client credentials (or assigned other authentication requirements), the client MUST authenticate with the authorization server as described in Section 3.2.1 |
| 6749 | 4.1.3   | The authorization server MUST<br />require client authentication for confidential clients or any client that was issued client credentials (or with other authentication requirements)<br />authenticate the client if client authentication is included<br />ensure that the authorization code was issued to the authenticated confidential client, or if the client is public ensure that the code was issued to "client_id" in the request<br />verify that the authorization code is valid<br />ensure that the "redirection_uri" parameter is present if the "redirect_uri" parameter was included in the initial authorization request as described in section 4.1.1, and if included ensure that their values are identical. |

