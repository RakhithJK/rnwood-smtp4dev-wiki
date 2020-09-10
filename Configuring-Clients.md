Any programs that you want to send mail to smtp4dev need to be configured so that they send mail via SMTP to the host/address and port number where smtp4dev is running.


Look for the following options in your program/platform
|Option|Value |
|-|-|
|SMTP hostname |`localhost` if client is on same machine as smtp4dev<br/>or the DNS name of the machine where smtp4dev is running e.g. `mymachine.mydomain`.|
| SMTP port number | `25` | 
| SSL/TLS, Secure connection, Encryption | `Off` or `Not required` etc |
| Authentication | `Off`. See below.
| Username/Password | `<Empty>`. Authentication is not required, but will be accepted if your client insists on their configuration


This is valid for the default configuration for smtp4dev. [See configuration information](Configuration) for information on how to check and change this including how to enable SSL/TLS.
