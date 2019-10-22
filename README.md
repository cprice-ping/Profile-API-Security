# Profile-API-Security
This Profile is used to demonstrate PingIntelligence [and PingDataGovernance] using PingAccess as the API Proxy.

It can be used to show the front-end aspects of Ping's API Security solutions with the Ping products to the side of the API integration layer(s).

Pre-requirements:
* PingIntelligence ABS \ Data Lake (Cloud PoC \ Docker PoC \ manual build -- https://github.com/cprice-ping/Profile-API-Security-Backend)
* P14C Environment \ Users \ Connection (Used as the Token Provider for PA)

Profile Configuration:
This profile contains the following services:

* PingAccess -- used to define and proxy API calls
* PI - AAD -- Used to create the PI API Definitions automatically
* PI - ASE -- API Security Enforcer that PA is communicating with

Environment Variables in `./env_vars` can be used to configure this stack to your instances of PI ABS and P14C.

## Adding and Protecting an API with PI
With this setup, the configuration of the APIs for PI is handled in the Application definition in PA.

[Note: Familiarity in creating and configuring an Application in PA is assumed]

2 P14C Endpoints are pre-configured to show examples of how to define an Application.

Credentials for PingAccess Admin UI -- `https:{{pa_host}}:9000`

`Administrator` \ `2FederateM0re`

1) Create a new API Application 
2) Add the PI Sideband rule to the Policy
3) In the Description of the Application add the proper JSON object for AAD

Sample JSON Description:

```
{
    "ping_ai": true,
    "ping_host": "*",
    "ping_url": "/mgmt",
    "ping_login": "",
    "ping_cookie": "",
    "apikey_qs": "",
    "apikey_header": "",
    "ping_decoy": false,
    "oauth2_access_token": true,
    "ping_blocking": true
}
```

AAD runs every 10 minutes and the Container logs will show if the Discovery was performed correctly:

```
pi-aad_1      | 2019-09-11 16:12:00 INFO  - starting pre-conversion stage
pi-aad_1      | 2019-09-11 16:12:00 INFO  - pre-conversion stage finished
pi-aad_1      | 2019-09-11 16:12:00 INFO  - getting discovered apis from pingaccess
pi-aad_1      | 2019-09-11 16:12:00 INFO  - getting api summary from pingAccess
pi-aad_1      | 2019-09-11 16:12:00 INFO  - getting existing apis from ase
pi-aad_1      | 2019-09-11 16:12:00 INFO  - getting current api list from ase
pi-aad_1      | 2019-09-11 16:12:00 INFO  - converting pingaccess discovered apis to ase api format
pi-aad_1      | 2019-09-11 16:12:00 INFO  - pushing to ase
pi-aad_1      | 2019-09-11 16:12:00 INFO  - wrote api json body for api P14C Mgmt API to file /opt/pingidentity/aad/data/conversion/current/pingaccess/P14C Mgmt API/new_ase_api_P14C Mgmt API.json
pi-aad_1      | 2019-09-11 16:12:00 INFO  - pushing new api P14C Mgmt API: {
pi-aad_1      |   "api_metadata" : {
pi-aad_1      |     "protocol" : "https",
pi-aad_1      |     "url" : "/mgmt",
pi-aad_1      |     "hostname" : "*",
pi-aad_1      |     "cookie" : "",
pi-aad_1      |     "cookie_idle_timeout" : "",
pi-aad_1      |     "logout_api_enabled" : false,
pi-aad_1      |     "cookie_persistence_enabled" : false,
pi-aad_1      |     "oauth2_access_token" : true,
pi-aad_1      |     "apikey_qs" : "",
pi-aad_1      |     "apikey_header" : "",
pi-aad_1      |     "enable_blocking" : true,
pi-aad_1      |     "login_url" : "",
pi-aad_1      |     "api_mapping" : {
pi-aad_1      |       "internal_url" : ""
pi-aad_1      |     },
pi-aad_1      |     "api_pattern_enforcement" : {
pi-aad_1      |       "protocol_allowed" : "",
pi-aad_1      |       "http_redirect" : {
pi-aad_1      |         "response_code" : "",
pi-aad_1      |         "response_def" : "",
pi-aad_1      |         "https_url" : ""
pi-aad_1      |       },
pi-aad_1      |       "methods_allowed" : [ ],
pi-aad_1      |       "content_type_allowed" : "",
pi-aad_1      |       "error_code" : "",
pi-aad_1      |       "error_def" : "",
pi-aad_1      |       "error_message_body" : ""
pi-aad_1      |     },
pi-aad_1      |     "flow_control" : {
pi-aad_1      |       "client_spike_threshold" : "0/second",
pi-aad_1      |       "server_connection_queueing" : false
pi-aad_1      |     },
pi-aad_1      |     "api_memory_size" : "64mb",
pi-aad_1      |     "health_check" : false,
pi-aad_1      |     "health_check_interval" : 60,
pi-aad_1      |     "health_retry_count" : 4,
pi-aad_1      |     "health_url" : "/",
pi-aad_1      |     "servers" : [ ],
pi-aad_1      |     "decoy_config" : {
pi-aad_1      |       "decoy_enabled" : false,
pi-aad_1      |       "response_code" : 200,
pi-aad_1      |       "response_def" : "",
pi-aad_1      |       "response_message" : "",
pi-aad_1      |       "decoy_subpaths" : [ ]
pi-aad_1      |     },
pi-aad_1      |     "server_ssl" : false
pi-aad_1      |   }
pi-aad_1      | }
pi-aad_1      | 2019-09-11 16:12:00 INFO  - new api P14C Mgmt API push status from ase :{
pi-aad_1      |     "status": "success",
pi-aad_1      |     "status_message": "api P14C Mgmt API added successfully"
pi-aad_1      | }
```


