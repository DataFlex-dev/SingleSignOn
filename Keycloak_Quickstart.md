# How to install Keycloak

## Table of Contents
- [Prerequisites](#prerequisites)
- [Starting the docker container](#starting-the-docker-container)
- [Starting with docker compose](#starting-with-docker-compose)
- [Configuring Keycloak](#configuring-keycloak)
- [Creating Realm](#creating-realm)
- [Creating Account / User](#creating-account--user)
- [Creating Client](#creating-client)
- [DataFlex](#dataflex)
- [`oSso`](#osso)
- [`oSsoOpenIdTestProvider`](#ossoopenidtestprovider)

## Prerequisites
- You must have a valid docker installation
- The docker engine must be running

## Starting the docker container
To start the docker container with a valid keycloak installation run the following command: 
```
docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:25.0.2 start-dev
```
This command must be run inside of a terminal / command prompt. If you have an administrator account, the terminal / cmd must be run as an administrator.

## Starting with docker compose
The project also includes a `docker-compose.yml` file in the root of the project.

To start Keycloak with that file, run this command from the project root:
```
docker compose up -d
```

This starts Keycloak in the background.

Useful commands:
```
docker compose logs -f keycloak
docker compose down
```

Note: the compose file maps Keycloak to port `8082`, so when using docker compose you must open:
```
http://localhost:8082/admin
```

## Configuring Keycloak
Before we can use the keycloak installation we must do some configuration.

First, go to the following link inside of an browser:
http://localhost:8080/admin .
Here we can login with our preset admin login. If you've used the command above this is: 
```
Username: admin
Password: admin
```

If you started Keycloak with `docker compose`, use `http://localhost:8082/admin` instead.

### Creating Realm
Once successfully logged in we must create a Realm. This will be our whole enviroment. 
- In the top left select the dropdown, that says keycloack and click on "Create realm".
- Give the Realm a name and leave the rest as is. (Make sure that "Enabled" is true)
- Click on "Create"
- Validate that you're now in the just created Realm by looking in the dropdown in the top-left corner.

### Creating Account / User
When we've created the Realm, we also need an user that can login with the SSO. We do this also inside of the new Realm.

- Go to "Users" on the left hand side.
- Click "Create new user".
- Give the user at least an "Username", but optionally more.
- Click "Create".
- Go to the "Credentials" tab.
- Click "Set password".
- Fill in the password twice and set "Temporary" to off.
- Click on "Save" and then "Save password".

### Creating Client
Once we have an user inside of an Realm, we must also create the Client application. This is also inside of the just created Realm.

- Go to "Clients" on the left hand side.
- Click "Create client".
- Fill in at least the "Client ID".
- Click "Next".
- Enable the "Client authentication" slider.
- Make sure that "Standard flow" is checked and that we uncheck "Direct access grants".
- Click "Next"
- Here we need to fill in the "Web origins" and the "Valid redirect URIs". This can be the following for test purposes (where you replace {webappname} with the name of the webapp):
```
Valid redirect URIs : http://localhost/{webappname}/*
Web origins : http://localhost/{webappname}
```
- Finally click "Save".
- Once created the "Client Secret" can be found in the Credentials tab. (The client ID is what we filled in ourselfs)

### DataFlex
Finally we can connect the keycloak to our webapp by changing the following properties:
- Change the `C_SSO_PROVIDER_OPENIDDEMO` constant to the Client-ID
### `oSso`
- Set the `psHomeURL` to the url of the DF application and the `psCallbackURLLogin` and `psCallbackURLLogout` to the url of the webapp extended with "/SSOCallback/Login" and "/SSOCallback/Login" respectively.
### `oSsoOpenIdTestProvider`
- Check the following: `psSSOHost`. It should be filled in correctly, if not change to the url off the sso provider.
- Change `psIdentityClientID` and `psIdentityClientSecret` to the values that you get from the admin console (Client ID and Client Secret)
- Check all the endpoint properties, these exist of `realms/{realmname}/protocol/....`. Here {realmname} must be changed to the name of the realm that you created. Everything else should be correct (the path of the endpoints).

Once done with the above, you can re-run / compile the web application again. And go the the webapp in your browser. This should auto-redirect you to the sso login page and you can login with the user created above.

Note that for the first time you will be prompted to fill in more data about the user / account. 
