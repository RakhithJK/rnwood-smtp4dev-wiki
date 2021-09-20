smtp4dev configuration can be performed via the `appsetting.json` file, environment variables and command line arguments. 

Some recently added options may not be reflected here.
For the latest information run smtp4dev with `--help` for information or see the `appsettings.json` file.

## Configuration Files

You can find the configuration file at `<installlocation>\appsettings.json`. This file is included in every release and so may be overwritten when you update. To avoid this, create a configuration file at `{AppData}/smtp4dev/appsettings.json` (see below) and make your customisations there.

Version 3.1.2 onwards will automatically reload and apply any edits to the configuration file without restarting. For previous versions you will need to restart the app to get the new changes to apply.

```jsonc
{
    "Environment": "Production",
    "Logging": {
        "IncludeScopes": false,
        "Debug": {
            "LogLevel": {
                "Default": "Warning"
            }
        },
        "Console": {
            "LogLevel": {
                "Default": "Warning"
            }
        }
    },

  //SMTP4DEV settings
  //
  //Values specified here can be overriden in the following places (in order of processing - last wins)
  // - ~/appsettings.{Environment}.json - Where {Environment} is from ASPNETCORE_ENVIRONMENT env var
  // - {AppData}/smtp4dev/appsettings.json - Where {AppData} is APPDATA env var on Windows or XDG_CONFIG_HOME on non-Windows
  // - Environmment variables in format "ServerOptions:HostName" (Windows) or "ServerOptions__HostName" (Other platforms)
  // - Command line arguments - Specify --help for documentation.

  "ServerOptions": {

    //Specifies the virtual path from web server root where SMTP4DEV web interface will be hosted. e.g. "/" or "/smtp4dev"
    //Default value: "/"
    "BasePath": "/",

    //Specifies the server hostname. Used in auto-generated TLS certificate if enabled.
    //Default value: "localhost"
    "HostName": "localhost",

    //Set the port the SMTP server listens on. Specify 0 to assign automatically
    //Default value: 25
    "Port": 25,

    //Specifies if remote connections will be allowed to the SMTP server
    //Default value: true
    "AllowRemoteConnections": true,

    //Specifies the path where the database will be stored relative to APPDATA env var on Windows or XDG_CONFIG_HOME on non-Windows. Leave value empty to use an in memory database.
    //Default value: "database.db"
    "Database": "database.db",

    //Specifies the number of messages to keep
    //Default value: 100
    "NumberOfMessagesToKeep": 100,

    //Specifies the number of sessions to keep
    //Default value: 100
    "NumberOfSessionsToKeep": 100,

    //Specifies the TLS mode to Use
    // Value        Description
    // None         Off
    // StartTls     On demand if client supports STARTTLS. 
    // ImplicitTls  TLS as soon as connection is established.
    //Default value: "None"
    "TlsMode": "None",

    //Specifies the TLS certificate to use if TLS is enabled/requested. Leave value empty to use an auto-generated self-signed certificate (then see console output on first startup)
    //Default value: ""
    "TlsCertificate": ""
  },

  "RelayOptions": {

    //Sets the name of the SMTP server that will be used to relay messages or \"\" if messages should not be relayed
    //Default value: ""
    "SmtpServer": "",

    //Sets the port number for the SMTP server used to relay messages.
    //Default value: 25
    "SmtpPort": 25,

    //Specifies a list of recipient addresses for which messages will be relayed. An empty list means that no messages are relayed.
    //Default value: []
    "AllowedEmails": [],

    //Specifies the address used in MAIL FROM when relaying messages. (Sender address in message headers is left unmodified). The sender of each message is used if not specified.
    //Default value: ""
    "SenderAddress": "",

    //The username for the SMTP server used to relay messages. If \"\" no authentication is attempted.
    //Default value: ""
    "Login": "",

    //The password for the SMTP server used to relay messages
    "Password": ""
  }
}
```

## Environment Variables

All the values from `appsettings.json` can be overriden by environment variables.

Set environmment variables in format 
* `"ServerOptions:HostName"` (Windows) 
* or `"ServerOptions__HostName"` (Other platforms)

## Command Line Options

```text
      -h, --help, -?             Shows this message and exits
      --urls=VALUE           The URLs the web interface should listen on. For
                               example, http://localhost:123. Use `*` in place
                               of hostname to listen for requests on any IP
                               address or hostname using the specified port and
                               protocol (for example, http://*:5000)
      --hostname=VALUE       Specifies the server hostname. Used in auto-
                               generated TLS certificate if enabled.
      --allowremoteconnections
                             Specifies if remote connections will be allowed to
                               the SMTP server.
      --smtpport=VALUE       Set the port the SMTP server listens on. Specify 0
                               to assign automatically
      --db=VALUE             Specifies the path where the database will be
                               stored relative to APPDATA env var on Windows or
                               XDG_CONFIG_HOME on non-Windows. Leave value empty to
                               use an in memory database.
      --messagestokeep=VALUE Specifies the number of messages to keep
      --sessionstokeep=VALUE Specifies the number of sessions to keep
      --tlsmode=VALUE        Specifies the TLS mode to use. 
                              Value        Description
                              None         Off
                              StartTls     On demand if client supports STARTTLS. 
                              ImplicitTls  TLS as soon as connection is established.
      --tlscertificate=VALUE Specifies the TLS certificate to use if TLS is
                               enabled/requested. Leave value empty to use an auto-
                               generated self-signed certificate (then see
                               console output on first startup)
      --basepath=VALUE       Specifies the virtual path from web server root
                               where SMTP4DEV web interface will be hosted. e.g.
                                "/" or "/smtp4dev"
      --relaysmtpserver=VALUE
                             Sets the name of the SMTP server that will be used
                               to relay messages or leave value empty if messages should not
                               be relayed
      --relaysmtpport=VALUE  Sets the port number for the SMTP server used to
                               relay messages
      --relayallowedemails=VALUE
                             A comma separated list of recipient addresses for
                               which messages will be relayed. An empty list
                               means that no messages are relayed
      --relaysenderaddress=VALUE
                             Specifies the address used in MAIL FROM when
                             relaying messages. (Sender address in message
                             headers is left unmodified). The sender of each
                             message is used if not specified.
      --relayusername=VALUE  The username for the SMTP server used to relay
                             messages. If value empty no authentication is attempted
      --relaypassword=VALUE  The password for the SMTP server used to relay
                             messages
      --imapport=VALUE       Specifies the port the IMAP server will listen on -
                             allows standard email clients to view/retrieve
                             messages
      --nousersettings       Skip loading of appsetttings.json file in %APPDATA%
      --recreatedb           Recreates the DB on startup if it already exists
```