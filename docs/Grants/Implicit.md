# Grants - Implicit

**<u>Request Parameters:</u>**

- **response_type** => required MUST be set to "token"
- **client_id** => required
- **redirect_uri** => optional
- **scope** => optional
- **state** => recommended SHOULD be used for preventing cross-site request forgery as described in section 10.12

**<u>Response Parameters:</u>**

- **access_token** => required
- **token_type** => required
- **expires_in** => recommended, if omitted, the authorization server SHOULD provide the expiration time via other means or document the default value
- **scope** => optional if identical to the scope requested by the client; otherwise required
- **state** => required if present in request; must be the exact value as received in the require

**<u>Error Parameters:</u>**

- **error** => required (*invalid_request*, *unauthorized_client*, *access_denied*, *unsupported_response_type*, *invalid_scope*, *server_error*, *temporarily_unavailable*)
- **error_description** => optional
- **error_uri** => optional
- **state** => required if present in request; must be the exact value as received in the require

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 4.2.1   | The authorization server MUST verify that the redirection URI to which it will redirect the access token matches a redirection URI registered by the client as described in section 3.1.2 |
| 6749 | 4.2.2   | The authorization server MUST NOT issue a refresh token      |
| 6749 | 4.2.2   | The client MUST ignore unrecognized response parameters      |
| 6749 | 4.2.2   | The authorization server SHOULD document the size of any value it issues |
| 6749 | 4.2.2.1 | If the request fails due to a missing, invalid, or mismatching redirection URI, or if the client identifier is missing or invalid, the authorization server SHOULD inform the resource owner of the error |
| 6749 | 4.2.2.1 | If the request fails due to a missing, invalid, or mismatching redirection URI, or if the client identifier is missing or invalid, the authorization server MUST NOT automatically redirect the user-agent to the invalid redirection URI |

