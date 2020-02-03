# Grants - Client Credentials

**<u>Request Parameters:</u>**

- **grant_type** => required MUST be set to "client_credentials"
- **scope** => optional

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 4.4     | The client credentials grant type MUST only be used by confidential clients |
| 6749 | 4.4.2   | The client MUST authenticate with the authorization server as described in Section 3.2.1 |
| 6749 | 4.4.2   | The authorization server MUST authenticate the client        |
| 6749 | 4.4.3   | A refresh token SHOULD NOT be included                       |

