# GraphQL Server for Tel-Aviv Municipal CRM 
  
[![Build Status](https://travis-ci.org/TLVMuni/graphql-crm-server.svg?branch=master)](https://travis-ci.org/TLVMuni/graphql-crm-server) 
<b>This is a production repository. Be careful to update the master branch of this repository because it is published as deployment source for Azure App Service https://tlvgraphql.azurewebsites.net</b>

## How to use
Launch Graph<i>i</i>ql interface for this server via <a href='https://tlvgraphql.azurewebsites.net/graphiql'>/graphiql</a> endpoint. You shoud authenticate youself as TLV Digitel customer. Explore GraphQL CRM schema from 'Docs' tab on upper rigth. On the left side of Graph<i>i</i>ql you can compose various GraphQL queries. You can enrich these queries with variables prepared on down left bar. 
<br>When ready, copy the query (possible with the variables) to your client and use the GraphQL tool appropriate for your language. We have a <a href='https://github.com/TLVMuni/graphiql-crm' target='_blank'>sample</a> of such client written in React.
<br> The actual endpoint of this service for GraphQL POSTs is <code>/graphql</code> (without <i>i</i>). Other HTTP verbs (GET, OPTIONS, PUT etc. are not served by GraphQL. Use (authenticated) mutations for updates, queries for reads. 

## How this GraphQL Server works
This is reference implementation of GraphQL Server for TLV CRM exposed by its <a href='https://apiportal.tel-aviv.gov.il/docs/services/597f1d1e9f9e5306143951da/operations/597f1d409f9e5306143951db' target='_blank'>API</a>. It is implemented with Apollo NodeJS packages. Invoking GraphQL server requires the authentication that could be performed against the <a href='https://apiportal.tel-aviv.gov.il/docs/services/59774fe39f9e531550416402/operations/597efe8e9f9e5306143951d1' target='_blank'>TLV authentication endpoint</a> (better be called <code>/me</code>) or interactively from <a href='https://tlvauth.azurewebsites.net/logintoken.html' target='_blank'>this page<a>. Currently, the authentication is based on the records of registered Digitel customers. The obtained authentication token (JWT) should be passed to GraphQL server with Authorization header, however, GraphQL Server does not validate the passed token. It just forwards the token down to the wrapped API. Since CRM API is actually managed by Azure API Management, the token is validated there in the policy associated with API.

## Notes on hosting
This site is tested to run under NodeJS ver. 8.9.0/1. When running on Azure hosting, the requirements for NodeJS version are expressed in Application Settings section of Azure Portal: <br>
<code>WEBSITE_NODE_DEFAULT_VERSION = 8.9.0</code>
<br>On Azure this site is hosted under IIS with iisnode. By default, <i>iisnode</i> ignores scripts in <i>package.json</i> and assumes the main entry point to the site is <i>server.js</i> as specified in <i>web.config</i>. According to this defaults, <i>server.js</i> of this project serves as a hook to run JS code within babel. This configuration is mirrored in included package.json and could be used for local runs:
<code>npm run start</code>
The actual code for the site is inside App.js.
