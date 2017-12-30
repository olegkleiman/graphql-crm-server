# GraphQL Server for Tel-Aviv Municipal CRM [![Build Status](https://travis-ci.org/TLVMuni/graphql-crm-server.svg?branch=master)](https://travis-ci.org/TLVMuni/graphql-crm-server)
<b>This is a production repository. Be careful to update the master branch of this repository because it is published as deployment source for Azure App Service https://tlvgraphql.azurewebsites.net</b>

## How this GraphQL Server works
This is reference implementation of GraphQL Server for TLV CRM exposed by its API. It is implemented with Apollo NodeJS packages. Invoking GraphQL server requires the authentication that could be performed against the TLV authentication endpoint. Currently the authentication is based on the records od registered Digitel customers. The obtained authentication token (JWT) should be passed to GraphQL server with Authorization header, however, GraphQL Server does not validate the passed token. It just forwards the token down to the wrapped API. Since CRM API is actually managed by Azure API Management, the token is valited there in the policy associated with API.

## Notes on hosting
This site is tested to run under NodeJS ver. 8.9.0/1. When running on Azure hosting, the requirements for NodeJS version are expressed in Application Settings section of Azure Portal: <br>
<code>WEBSITE_NODE_DEFAULT_VERSION = 8.9.0</code>
<br>On Azure this site is hosted under IIS with iisnode. By default, <i>iisnode</i> ignores scripts in <i>package.json</i> and assumes the main entry point to the site is <i>server.js</i> as specified in <i>web.config</i>. According to this defaults, <i>server.js</i> of this project serves as a hook to run JS code within babel. This configuration is mirrored in included package.json and could be used for local runs:
<code>npm run start</code>
The actual code for the site is inside App.js.
