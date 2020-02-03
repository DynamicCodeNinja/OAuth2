# Grants - Resource Owner Password Credentials

**<u>Request Parameters:</u>**

- **grant_type** => required MUST be set to "password"
- **username** => required
- **password** => required
- **scope** => optional

| RFC  | Section | Text                                                         |
| ---- | ------- | ------------------------------------------------------------ |
| 6749 | 4.3.1   | The client MUST discard the credentials once an access token has been obtained |
| 6749 | 4.3.2   | If the client type is confidential or the client was issued client credentials (or assigned other authentication requirements), the client MUST authenticate with the authorization server as described in section 3.2.1 |
| 6749 | 4.3.2   | The authorization server MUST<br />require client authentication for confidential clients or for any client that was issued client credentials (or with other authentication requirements)<br />authenticate the client if client authentication is included<br />validate the resource owner password credentials using its existing password validation algorithm |
| 6749 | 4.3.2   | The authorization server MUST protect the endpoint against brute force attacks |

