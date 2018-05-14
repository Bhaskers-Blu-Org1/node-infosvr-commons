# README

Objective of this module is to provide re-usable functionality and utilities for interacting with an IBM Information Server environment using NodeJS.

# Utilities

## createInfoSvrAuthFile.js

Creates an authorisation file for use with connecting to an Information Server environment.  Usage:

```shell
node ./createInfoSvrAuthFile.js
	-u <user>
	[-f <file>]
	[-p <password>]
```

By default (if not specified using the optional parameters), will create a file under `~/.infosvrauth` and will prompt the user to enter the password on the command-line.  The utility makes use of Information Server's `encrypt.sh` script, so this should be run from an Information Server node (a tier with `.../ASBNode/bin/encrypt.sh`).  Once created, the authorisation file can then be copied to any other system and used from any other system to provide the connectivity details necessary to connect remotely to the Information Server environment.

If executing the script on the Information Server system itself is not an option, but you still want to create an authorisation file, your best bet is to manually create one using the following format:

```properties
user=isadmin
password=
domain=is-server.ibm.com:9445
server=IS-SERVER.IBM.COM
```

The password can be left blank (most utilities that make use of this authorisation file request the password to be provided separately anyway, either as another parameter or via a command-line prompt).  Ensure, however, that the `domain` property refers to the correct FQDN of your domain-tier host as well as the port, and that the `server` property refers to your engine tier's FQDN. 

# API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

## ibm-iis-commons

Re-usable functions for inferring properties of and interacting with an IBM InfoSphere Information Server environment

## RestConnection

RestConnection class -- for handling connectivity to REST APIs

**Parameters**

-   `username`  
-   `password`  
-   `host`  
-   `port`  
-   `maxSockets`  

**Examples**

```javascript
const commons = require('ibm-iis-commons');
const restConnect = new commons.RestConnection('isadmin', 'isadmin-password', 'localhost', '9445');
const igcrest = require('ibm-igc-rest');
igcrest.setConnection(restConnect);
```

### constructor

Sets up a REST API connection

**Parameters**

-   `username` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** username to use when authenticating to REST API
-   `password` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** password to use when authenticating to REST API
-   `host` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** hostname of the domain tier
-   `port` **int** port number of the domain tier
-   `maxSockets` **int?** the maximum number of sockets to support concurrently on the host

### auth

Get the authentication object for a REST connection

Returns **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** a hash of 'user' and 'pass' basic authentication details

### host

Get the hostname for the REST connection

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### port

Get the port for the REST connection

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### connection

Get the qualified connection details for the REST connection (host:port)

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### baseURL

Get the fully-qualified base SSL URL for the REST connection

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### agent

Get the https.Agent object for the REST connection

Returns **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 

### sessionStatus

Retrieves indication of whether a session is open (true) or not (false)

Returns **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** 

### markSessionOpen

Marks that a session has been opened

### markSessionClosed

Marks that the session has been closed

## EnvironmentContext

EnvironmentContext class -- for encapsulating the context of an Information Server environment (NOTE: always run from Engine tier)

**Parameters**

-   `installLocation`  
-   `authFile`  

**Examples**

```javascript
const commons = require('ibm-iis-commons');
const envCtx = new commons.EnvironmentContext();
console.log("Host details: " + envCtx.domainHost + ":" + envCtx.domainPort);
console.log("Version     : " + envCtx.currentVersion);
console.log("Patches     : " + envCtx.installedPatches);
console.loc("$DSHOME     : " + envCtx.dshome);
```

### constructor

Sets up everything we can determine about the environment from the current system

**Parameters**

-   `installLocation` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)?** specifies the root of the installation ('/opt/IBM/InformationServer' by default)
-   `authFile` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)?** specifies the location of an authorisation file from which information can be retrieved (if available)

### ishome

Get the installation location of Information Server

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### dshome

Get the installation location of DataStage

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### asbhome

Get the installation location of the ASBNode

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### istool

Get the fully-qualified location of the istool command

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### currentVersion

Get the version of the Information Server installation

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### installedPatches

Get an array of installed patches on the Information Server environment

Returns **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>** 

### domainHost

Get the hostname of the Information Server domain (services) tier

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### domainPort

Get the port number of the Information Server domain (services) tier

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### domain

Get fully-qualified Information Server domain (services) tier information -- host:port

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### engine

Get the hostname of the Information Server engine tier

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### createAuthFileFromParams

Creates an authorisation file that can be used with most Information Server CLI tools
(so that passwords are not shown in-the-clear on the command line) -- based on the 
values provided
NOTE: this must be run from the Information Server environment directly (cannot be run remotely)

**Parameters**

-   `username` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** username to use for authentication
-   `password` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** password to use for authentication
-   `file` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** file into which to store the details

### addRemoteConnectionDetailsToAuthFile

Adds remote connection details to an existing authorisation file, that can then be used for 
connecting to an Information Server system remotely (requires SSH and key-based authentication
to be pre-configured, or a local Docker container)

**Parameters**

-   `file` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** authorisation file into which to append the remote connection details
-   `accessType` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** type of access, either DOCKER or SSH
-   `username` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** OS username used for remote access
-   `privateKey` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** SSH private key file used for authentication
-   `hostOrContainer` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** hostname (or IP) of the remote Information Server system (when accessType is SSH), or container name (when accessType is DOCKER)
-   `port` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)?** SSH port number for the remote Information Server system

### authFile

Get the fully-qualified location of the authorisation file (if any)

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### authFile

Set the location of the authorisation file

**Parameters**

-   `file`  {string}

### username

Get the username, from the authorisation file if needed

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### password

Get the user's password, from the authorisation file if needed

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### remoteConnectionString

Get the remote connection string, from the authorisation file if needed

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### remoteCopyString

Get the remote copy string, from the authorisation file if needed

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### getRestConnection

-   **See: module:ibm-iis-commons~RestConnection**

Get a RestConnection object allowing REST API's to connect to this environment

**Parameters**

-   `password` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** unencrypted password to use for REST connection (other details taken from authorisation file automatically)
-   `maxSockets` **int?** the maximum number of sockets to support concurrently on the host

Returns **[RestConnection](#restconnection)** 

### runInfoSvrCommand

Run the provided command on the Information Server environment

**Parameters**

-   `command` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### copyFile

Copy the provided source file to the target location on the Information Server environment

**Parameters**

-   `source` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 
-   `target` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

### removeFile

Remove the specified file from the Information Server environment

**Parameters**

-   `file`  
-   `source` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 
-   `target` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 
