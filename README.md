<link rel="stylesheet" type="text/css" href="style.css">

# newsgears-app

newsgears is a multi-user, self-hosted all-in-one RSS reader/aggregator platform.

The NewsGears platform is based on four container types, thus this repository contains four submodules:

newsgears-api: provides HTTP-based REST access to the core subscription management capabilities of the entire platform

newsgears-engine: performs scheduled/periodic tasks, such as purging old/read posts, etc.

newsgears-client: a browser application to read and share syndicated web feed content.  

newsgears-broker: a platform infrastructure component built to facilitate real-time communication between client and server components.

## To self-host NewsGears:

## 1. Setup docker-compose.yml:

Create a docker-compose.yml file from the same provided in this repository.

```
cp docker-compose.yml.sample docker-compose.yml 
```

#### (Optional) If you want to enable OAUTH2 via Google:
- ```SPRING_SECURITY_OAUTH2_CLIENT_REGISTRATION_GOOGLE_CLIENTID=@null```
- ```SPRING_SECURITY_OAUTH2_CLIENT_REGISTRATION_GOOGLE_CLIENTSECRET=@null```
- ```SPRING_SECURITY_OAUTH2_CLIENT_REGISTRATION_GOOGLE_REDIRECTURI=http://localhost:8080/oauth2/callback/{registrationId}```
- ```SPRING_SECURITY_OAUTH2_CLIENT_REGISTRATION_GOOGLE_SCOPE=email,profile```

Get your own values for client Id/client secret from Google and plug them in to these variables in ```docker-compose.yml```. 

The value of the OAuth2 redirect URI should be:

```
http://localhost:8080/oauth2/callback/{registrationId}
```

The value of the ```scope``` property must be ```email,profile```, regardless of the OAuth2 provider.

If you don't want to use OAuth2, you'll have to go through the account registration process in order to login.

<hr>

## 2. Quick-start using pre-built containers:

If you don't want to do development, just start NewsGears using pre-built containers:

```
docker-compose up
```

<hr>

## 3. For local development: 

If you don't want to use the pre-built containers (i.e., you want to make custom code changes and build your own containers), then use the following instructions.  

### Setup command aliases: 

A script called `build_module.sh` is provided to expedite image assembly.  Setup command aliases to run it to build the required images after you make code changes: 

```
alias ng-api='./build_module.sh newsgears-api'
alias ng-engine='./build_module.sh newsgears-engine'
alias ng-broker='./build_module.sh newsgears-broker'
alias ng-client='./buid_client.sh'
```

#### Alternately, setup aliases build debuggable containers: 

```
alias ng-api='./build_module.sh newsgears-api --debug 45005'
alias ng-engine='./build_module.sh newsgears-engine --debug 55005'
alias ng-broker='./build_module.sh newsgears-broker --debug 65015'
alias ng-client='./buid_client.sh'
```

*Debuggable containers pause on startup until a remote debugger is attached on the specified port.*

### Build and run: 

#### Run the following command in the directory that contains ```newsgears-app```:  

```
ng-api && ng-engine && ng-broker && ng-client && docker-compose up
```

Boot down in the regular way, by using ```docker-compose down``` in the ```newsgears-app``` directory.

<hr> 

You can also use the `ng-api`, `ng-engine`, `ng-broker`, and `ng-client` aliases to rebuild the containers (i.e., to deploy code changes).

```
$ ng-api # rebuild the API server container 
$ ng-engine # rebuild the engine server container 
$ ng-broker # reubild the broker server container 
$ ng-client # rebuild the client server container 
```

Restart after each. 
