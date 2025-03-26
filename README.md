**OneLogin Go Lang**

**[Introduction]{.underline}**

This is the Onelogin SDK, a Go package that provides a convenient
interface for interacting with Onelogin\'s API. The SDK simplifies the
integration process by providing developers an easy-to-use tool for
managing authentication, making API requests, and handling responses.

**[Installation]{.underline}**

To use the Onelogin SDK in your Go project, you need to have Go
installed and set up. Then, you can install the SDK using the go
get command:

**go get github.com/onelogin/onelogin-go-sdk**

**Requirements**

The SDK expects three environment variables to be set for
authentication:

- ONELOGIN_CLIENT_ID

- ONELOGIN_CLIENT_SECRET

- ONELOGIN_SUBDOMAIN

- ONELOGIN_TIMEOUT

These variables are used by the Authenticator for authentication with
the OneLogin API. The Authenticator retrieves an access token using
these credentials, which is then used for API requests. You can set
these variables directly in your shell or in the environment of the
program that will be using this SDK.

**In a Unix/Linux shell, you can use the export command to set these
variables:**

export ONELOGIN_CLIENT_ID=your_client_id

export ONELOGIN_CLIENT_SECRET=your_client_secret

export ONELOGIN_SUBDOMAIN=your_subdomain

export ONELOGIN_TIMEOUT=15

**In a Go program, you can use the os package to set these variables:**

os.Setenv(\"ONELOGIN_CLIENT_ID\", \"your_client_id\")

os.Setenv(\"ONELOGIN_CLIENT_SECRET\", \"your_client_secret\")

os.Setenv(\"ONELOGIN_SUBDOMAIN\", \"your_subdomain\")

os.Setenv(\"ONELOGIN_TIMEOUT\", 15)

**Note**: Please ensure these variables are set before attempting to use
the SDK to make API requests.

**[Getting started]{.underline}**

**[Usage]{.underline}**

Here\'s an example demonstrating how to use the Onelogin SDK:

package main

import (

\"fmt\"

\"github.com/onelogin/onelogin-go-sdk/v4/pkg/onelogin/models\"

\"github.com/onelogin/onelogin-go-sdk/v4/pkg/onelogin\"

)

func main() {

ol, err := onelogin.NewOneloginSDK()

if err != nil {

fmt.Println(\"Unable to initialize client:\", err)

return

}

userQuery := models.UserQuery{}

userList, err := ol.GetUsers(&userQuery)

if err != nil {

fmt.Println(\"Failed to get user:\", err)

return

}

fmt.Println(userList)

appQuery := models.AppQuery{}

appList, err := ol.GetApps(&appQuery)

if err != nil {

fmt.Println(\"Failed to get app list:\", err)

return

}

fmt.Println(\"App List:\", appList)

}

**Errors and Exceptions**

OneLogin\'s API can return 400, 401, 403 or 404 when there was any issue
executing the action. 

**[Methods]{.underline}**

1.  **[Apps]{.underline}**

**Method**

get_apps

**Endpoint**

**GET** /api/2/apps

**Query Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **name**   | The name or partial name of the app to search for. When |
|            | using a partial name you must append a wildcard \`\*\`  |
| string     |                                                         |
|            | e.g. GET /apps?name=workday\*                           |
+------------+---------------------------------------------------------+
| **conn     | Returns all apps based off a specific connector.        |
| ector_id** |                                                         |
|            |                                                         |
| number     |                                                         |
+------------+---------------------------------------------------------+
| **aut      | Returns all apps based of a given type.                 |
| h_method** |                                                         |
|            | - 0 - Password                                          |
| number     |                                                         |
|            | - 1 - OpenId                                            |
|            |                                                         |
|            | - 2 - SAML                                              |
|            |                                                         |
|            | - 3 - API                                               |
|            |                                                         |
|            | - 4 - Google                                            |
|            |                                                         |
|            | - 6 - Forms Based App                                   |
|            |                                                         |
|            | - 7 - WSFED                                             |
|            |                                                         |
|            | 8 - OpenId Connect                                      |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      
  -----------------------------------------------------------------------

**Method**

get_app_by_id

**Endpoint**

**GET** /api/2/apps/:id

**Resource Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **id**     | Set to the id of the app that you want to return.       |
| (required) |                                                         |
|            |                                                         |
| integer    |                                                         |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         SAML                      **-**

  200         OpenID Connect            

  401         Unauthorized              **-**

  404         Not Found                 
  -----------------------------------------------------------------------

**Method**

get_app_users

**Endpoint**

**GET** /api/2/apps/:app_id/users

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Reponse        **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

create_app

**Endpoint**

**POST** /api/2/apps

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **connector_id** | ID of the connector to base the app from.         |
| (required)       |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **name**         | The name of the app                               |
| (required)       |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Example:**

**Method**

update_app

**Endpoint**

**PUT** /api/2/apps/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Id**           | Set to the id of the app that you want to update. |
| (required)       |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_app_parameter

**Endpoint**

**DEL** /api/2/apps/:id/parameters/:parameter_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the app that you want to delete. |
| (required)       |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **parameter_id** | Set to the id of the parameter that you want to   |
| (required)       | delete.                                           |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  403         Forbidden                 

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

delete_app

**Endpoint**

**DEL** /api/2/apps/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the app that you want to delete. |
| (required)       |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

2.  **[App Rules]{.underline}**

**Method**

get_app_rules

**Endpoint**

**GET** /api/2/apps/:app_id/rules

**Resource Parameters**

+-------------+--------------------------------------------------------+
| Name/Type   | Description                                            |
+=============+========================================================+
| **app_id    | The id of the application that where the rules apply.  |
| (           |                                                        |
| required)** |                                                        |
|             |                                                        |
| integer     |                                                        |
+-------------+--------------------------------------------------------+

**Query Parameters**

+--------------+-------------------------------------------------------+
| Name/Type    | Description                                           |
+==============+=======================================================+
| **enabled**  | Defaults to true. When set to \`false\` will return   |
|              | all disabled rules.                                   |
| boolean      |                                                       |
+--------------+-------------------------------------------------------+
| **has        | Filters Rules based on their Conditions. Values       |
| _condition** | formatted as:, where name is the Condition to look    |
|              | for, and value is the value to find. Multiple filters |
| string       | can be declared by using a comma delimited list.      |
|              | Wildcards are supported in both the name and          |
|              | value fields.                                         |
|              |                                                       |
|              | *For example:*                                        |
|              |                                                       |
|              | Single filter. has_condition=has_role:123456          |
|              |                                                       |
|              | Multiple                                              |
|              | filters. has_condition=has_role:123456,status:1       |
|              |                                                       |
|              | Wildcard for conditions. has_condition=\*:123456      |
|              |                                                       |
|              | Wildcard for condition                                |
|              | values. has_condition=has_role:\*                     |
+--------------+-------------------------------------------------------+
| **has_cond   | Filters Rules based on their condition types.         |
| ition_type** |                                                       |
|              | Allowed values are:                                   |
| string       |                                                       |
|              | - **builtin** - actions that involve                  |
|              |   standard attributes                                 |
|              |                                                       |
|              | - **custom** - actions that involve custom attributes |
|              |                                                       |
|              | - **none** - no actions are defined                   |
|              |                                                       |
|              | *For example:*                                        |
|              |                                                       |
|              | Find Rules using custom user                          |
|              | attributes has_condition_type=custom                  |
|              |                                                       |
|              | Find Rules with no conditions has_condition_type=none |
+--------------+-------------------------------------------------------+
| **           | Filters Rules based on their Actions. Values          |
| has_action** | formatted as :, where name is the Action to look for, |
|              | and value is the value to find. Multiple filters can  |
| string       | be declared by using a comma delimited list.          |
|              | Wildcards are supported in both the name and          |
|              | value fields.                                         |
|              |                                                       |
|              | *For example:*                                        |
|              |                                                       |
|              | Single filter. has_action=set_licenses:123456         |
|              |                                                       |
|              | Multiple                                              |
|              | filters. has_action=set_groups:123456,set_usertype:\* |
|              |                                                       |
|              | Wildcard for actions. has_action=\*:123456            |
|              |                                                       |
|              | Wildcard for action                                   |
|              | values. has_action=set_userprincipalname:\*           |
+--------------+-------------------------------------------------------+
| **has_a      | Filters Rules based on their action types.            |
| ction_type** |                                                       |
|              | Allowed values are:                                   |
| string       |                                                       |
|              | - **builtin** - actions that involve                  |
|              |   standard attributes                                 |
|              |                                                       |
|              | - **custom** - actions that involve custom attributes |
|              |                                                       |
|              | - **none** - no actions are defined                   |
|              |                                                       |
|              | *For example:*                                        |
|              |                                                       |
|              | Find Rules with no actions has_action_type=none       |
+--------------+-------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Success                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_app_rule_by_id

**Endpoint**

**GET** /api/2/apps/:app_id/rules/:id

**Resource Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **app_id   | The id of the application that where the rules apply.   |
| (r         |                                                         |
| equired)** |                                                         |
|            |                                                         |
| integer    |                                                         |
+------------+---------------------------------------------------------+
| **id**     | The id of the app rule to locate.                       |
| (required) |                                                         |
|            |                                                         |
| integer    |                                                         |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Success                   **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

create_app_rule

**Endpoint**

**POST** /api/2/apps/:app_id/rules

**Request Parameters (Required)**

+----------------+-----------------------------------------------------+
| Name/Type      | Description                                         |
+================+=====================================================+
| **app_id**     | The id of the application that where the rules      |
|                | apply.                                              |
| integer        |                                                     |
+----------------+-----------------------------------------------------+
| **name**       | The name of the rule.                               |
|                |                                                     |
| String         |                                                     |
+----------------+-----------------------------------------------------+
| **Enabled**    | Indicates if the rule is enabled or not.            |
|                |                                                     |
| Boolean        |                                                     |
+----------------+-----------------------------------------------------+
| **match**      | Indicates how conditions should be matched.         |
|                |                                                     |
| String         | - **all** - Match all conditions                    |
|                |                                                     |
|                | **any** - Match any condition                       |
+----------------+-----------------------------------------------------+
| **position**   | Indicates the order of the rule. When \`null\` this |
|                | will default to last position.                      |
| Integer        |                                                     |
+----------------+-----------------------------------------------------+
| **conditions** | An array of conditions that the user must meet in   |
|                | order for the rule to be applied.                   |
| Array          |                                                     |
|                | - **source** - The source field to check. See [List |
|                |   Conditions](https://developers.one                |
|                | login.com/api-docs/2/app-rules/list-conditions) for |
|                |   possible values.                                  |
|                |                                                     |
|                | - **operator** - A valid operator for the selected  |
|                |   condition source. See [List Condition             |
|                |   Operators](https://developers.onelogin.com        |
|                | /api-docs/2/app-rules/list-condition-operators) for |
|                |   possible values.                                  |
|                |                                                     |
|                | **value** - A plain text string or valid value for  |
|                | the selected condition source. See [List Condition  |
|                | Values](https://developers.onelogin.                |
|                | com/api-docs/2/app-rules/list-condition-values) for |
|                | possible values.                                    |
+----------------+-----------------------------------------------------+
| **actions**    | An array of actions that will be applied to the     |
|                | users that are matched by the conditions.           |
| Array          |                                                     |
|                | - **action** - The action to apply. See [List       |
|                |   Actions](https://developers.one                   |
|                | login.com/api-docs/2/app-rules/list-conditions) for |
|                |   possible values.                                  |
|                |                                                     |
|                | - **value** - An array of strings. Only applicable  |
|                |   to provisioned and set\_\* actions. Items in the  |
|                |   array will be a plain text string or valid value  |
|                |   for the selected action. See [List Action         |
|                |   Values](https://developers.onelog                 |
|                | in.com/api-docs/2/app-rules/list-action-values) for |
|                |   possible values. In most cases only a single item |
|                |   will be accepted in the array.                    |
|                |                                                     |
|                | - **expression** - A regular expression to extract  |
|                |   a value. Applies to provisionable, multi-selects, |
|                |   and string actions.                               |
|                |                                                     |
|                | - **scriplet** - A hash containing scriptlet code   |
|                |   that returns a value. Scriptlets can not be       |
|                |   modified and the same hash should not be applied  |
|                |   to other applications.                            |
|                |                                                     |
|                | **macro** - A template to construct a value.        |
|                | Applies to default, string, and list actions.       |
+----------------+-----------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

update_app_rule

**Endpoint**

**PUT** /api/2/apps/:app_id/rules/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **app_id**       | The id of the application that where the rules    |
| (required)       | apply.                                            |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **Id**           | Set to the id of the rule that you want to        |
| (required)       | update.                                           |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters (Required)**

+----------------+-----------------------------------------------------+
| Name/Type      | Description                                         |
+================+=====================================================+
| **name**       | The name of the rule.                               |
|                |                                                     |
| String         |                                                     |
+----------------+-----------------------------------------------------+
| **Enabled**    | Indicates if the rule is enabled or not.            |
|                |                                                     |
| Boolean        |                                                     |
+----------------+-----------------------------------------------------+
| **match**      | Indicates how conditions should be matched.         |
|                |                                                     |
| String         | - **all** - Match all conditions                    |
|                |                                                     |
|                | **any** - Match any condition                       |
+----------------+-----------------------------------------------------+
| **position**   | Indicates the order of the rule. When \`null\` this |
|                | will default to last position.                      |
| Integer        |                                                     |
+----------------+-----------------------------------------------------+
| **conditions** | An array of conditions that the user must meet in   |
|                | order for the rule to be applied.                   |
| Array          |                                                     |
|                | - **source** - The source field to check. See [List |
|                |   Conditions](https://developers.one                |
|                | login.com/api-docs/2/app-rules/list-conditions) for |
|                |   possible values.                                  |
|                |                                                     |
|                | - **operator** - A valid operator for the selected  |
|                |   condition source. See [List Condition             |
|                |   Operators](https://developers.onelogin.com        |
|                | /api-docs/2/app-rules/list-condition-operators) for |
|                |   possible values.                                  |
|                |                                                     |
|                | **value** - A plain text string or valid value for  |
|                | the selected condition source. See [List Condition  |
|                | Values](https://developers.onelogin.                |
|                | com/api-docs/2/app-rules/list-condition-values) for |
|                | possible values.                                    |
+----------------+-----------------------------------------------------+
| **actions**    | An array of actions that will be applied to the     |
|                | users that are matched by the conditions.           |
| Array          |                                                     |
|                | - **action** - The action to apply. See [List       |
|                |   Actions](https://developers.one                   |
|                | login.com/api-docs/2/app-rules/list-conditions) for |
|                |   possible values.                                  |
|                |                                                     |
|                | - **value** - An array of strings. Only applicable  |
|                |   to provisioned and set\_\* actions. Items in the  |
|                |   array will be a plain text string or valid value  |
|                |   for the selected action. See [List Action         |
|                |   Values](https://developers.onelog                 |
|                | in.com/api-docs/2/app-rules/list-action-values) for |
|                |   possible values. In most cases only a single item |
|                |   will be accepted in the array.                    |
|                |                                                     |
|                | - **expression** - A regular expression to extract  |
|                |   a value. Applies to provisionable, multi-selects, |
|                |   and string actions.                               |
|                |                                                     |
|                | - **scriplet** - A hash containing scriptlet code   |
|                |   that returns a value. Scriptlets can not be       |
|                |   modified and the same hash should not be applied  |
|                |   to other applications.                            |
|                |                                                     |
|                | **macro** - A template to construct a value.        |
|                | Applies to default, string, and list actions.       |
+----------------+-----------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_app_rule

**Endpoint**

**DEL** /api/2/apps/:app_id/rules/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **app_id**       | The id of the application that where the rules    |
| (required)       | apply.                                            |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **Id**           | Set to the id of the rule that you want to        |
| (required)       | delete.                                           |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

list_app_rules_conditions

**Endpoint**

**GET** /api/2/apps/:app_id/rules/conditions

**Resource Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **app_id   | The id of the application that where the rules apply.   |
| (r         |                                                         |
| equired)** |                                                         |
|            |                                                         |
| integer    |                                                         |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Success                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_app_rule_operators

**Endpoint**

**GET** /api/2/apps/:app_id/rules/conditions/:condition_value/operators

**Resource Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **app_id   | The id of the application that where the rules apply.   |
| (r         |                                                         |
| equired)** |                                                         |
|            |                                                         |
| integer    |                                                         |
+------------+---------------------------------------------------------+
| **conditi  | The value for the selected condition.                   |
| on_value** |                                                         |
|            |                                                         |
| string     |                                                         |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Success                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_app_rule_condition_values

**Endpoint**

**GET** /api/2/apps/:app_id/rules/conditions/:condition_value/values

**Resource Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **app_id   | The id of the application that where the rules apply.   |
| (r         |                                                         |
| equired)** |                                                         |
|            |                                                         |
| integer    |                                                         |
+------------+---------------------------------------------------------+
| **conditi  | The value for the selected condition.                   |
| on_value** |                                                         |
|            |                                                         |
| string     |                                                         |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Success                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_app_rule_actions

**Endpoint**

**GET** /api/2/apps/:app_id/rules/actions

**Resource Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **app_id   | The id of the application that where the rules apply.   |
| (r         |                                                         |
| equired)** |                                                         |
|            |                                                         |
| integer    |                                                         |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Success                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

list_app_rules_action_values

**Endpoint**

**GET** /api/2/apps/:app_id/rules/actions/:action_value/values

**Resource Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **app_id   | The id of the application that where the rules apply.   |
| (r         |                                                         |
| equired)** |                                                         |
|            |                                                         |
| integer    |                                                         |
+------------+---------------------------------------------------------+
| **acti     | The value for the selected action.                      |
| on_value** |                                                         |
|            |                                                         |
| string     |                                                         |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Success                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

bulk_sort_app_rules

**Endpoint**

**PUT** /api/2/apps/:app_id/rules/sort

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

3.  **[Roles]{.underline}**

**Method**

get_roles

**Endpoint**

**GET** /api/2/roles

**Query Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **name**   | Optional. Filters by role name.                         |
|            |                                                         |
| string     |                                                         |
+------------+---------------------------------------------------------+
| **app_id** | Optional. Returns roles that contain this app ID.       |
|            |                                                         |
| integer    |                                                         |
+------------+---------------------------------------------------------+
| **         | Optional. Returns roles that contain this app name.     |
| app_name** |                                                         |
|            |                                                         |
| string     |                                                         |
+------------+---------------------------------------------------------+
| **fields** | Optional. Comma delimited list of fields to return.     |
|            |                                                         |
| string     | Valid options are apps, users, admins.                  |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Success                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

create_role

**Endpoint**

**POST** /api/2/roles

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Name**         | The name of the role.                             |
| (required)       |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **apps**         | A list of app IDs that will be assigned to        |
|                  | the role.                                         |
| array            |                                                   |
|                  | E.g. \[234, 567, 777\]                            |
+------------------+---------------------------------------------------+
| **users**        | A list of user IDs to assign to the role.         |
|                  |                                                   |
| array            |                                                   |
+------------------+---------------------------------------------------+
| **admins**       | A list of user IDs to assign as                   |
|                  | role administrators.                              |
| array            |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_role_by_id

**Endpoint**

**GET** /api/2/roles/:id

**Resource Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **Id**     | Set to the id of the role you want to return.           |
|            |                                                         |
| (required) |                                                         |
|            |                                                         |
| integer    |                                                         |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         SAML                      **-**

  401         Unauthorized              **-**

  404         Not Found                 
  -----------------------------------------------------------------------

**Method**

update_role

**Endpoint**

**PUT** /api/2/roles/:id

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **name**         | Name of the role.                                 |
| (required)       |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

get_role_apps

**Endpoint**

**GET** /api/2/roles/:id/apps

**Resource Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **         | Optional. Defaults to true. Returns all apps not yet    |
| assigned** | assigned to the role.                                   |
|            |                                                         |
| *          |                                                         |
| *boolean** |                                                         |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 
  -----------------------------------------------------------------------

**Method**

update_role_apps

**Endpoint**

**PUT** /api/2/roles/:id/apps

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **app_id**       | The complete list of app_id values to assign to   |
| (required)       | the role. To add or remove additional apps,       |
|                  | submit the complete list in your request. Don't   |
| array            | submit a partial list of app IDs, to add or       |
|                  | remove, in your request.                          |
|                  |                                                   |
|                  | For example: \[123, 456, 678\].                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_role_users

**Endpoint**

**GET** /api/2/roles/:id/users

**Request Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **name**      | Allows you to filter on first name, last             |
| (required)    | name, username, and email address.                   |
|               |                                                      |
| string        |                                                      |
+---------------+------------------------------------------------------+
| **include     | Optional. Defaults to false. Include users that      |
| _unassigned** | aren't assigned to the role.                         |
|               |                                                      |
| boolean       |                                                      |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 
  -----------------------------------------------------------------------

**Method**

add_role_users

**Endpoint**

**POST** /api/2/roles/:id/users

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set user_id values in array, for example: \[123,  |
| (required)       | 456, 678\]                                        |
|                  |                                                   |
| array            |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_role_users

**Endpoint**

**DEL** /api/2/roles/:id/users

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set user_id values in array, for example: \[123,  |
| (required)       | 456, 678\]                                        |
|                  |                                                   |
| array            |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_role_admins

**Endpoint**

**GET** /api/2/roles/:id/admins

**Request Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **name**      | Allows you to filter on first name, last             |
| (required)    | name, username, and email address.                   |
|               |                                                      |
| string        |                                                      |
+---------------+------------------------------------------------------+
| **include     | Optional. Defaults to false. Include users that      |
| _unassigned** | aren't assigned to the role.                         |
|               |                                                      |
| boolean       |                                                      |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 
  -----------------------------------------------------------------------

**Method**

add_role_admins

**Endpoint**

**POST** /api/2/roles/:id/admins

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set user_id values in array, for example: \[123,  |
| (required)       | 456, 678\]                                        |
|                  |                                                   |
| array            |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

delete_role_admins

**Endpoint**

**DEL** /api/2/roles/:id/admins

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set user_id values in array, for example: \[123,  |
| (required)       | 456, 678\]                                        |
|                  |                                                   |
| array            |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

delete_role

**Endpoint**

**DEL** /api/2/roles/:id

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the role you want to delete.     |
| (required)       |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

4.  **[Smart Hooks]{.underline}**

**Method**

list_hooks

**Endpoint**

**GET** /api/2/hooks

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_hook

**Endpoint**

**GET** /api/2/hooks/:id

**Resource Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **id**        | Set to the id of the Hook that you want to return.   |
|               |                                                      |
| required      |                                                      |
|               |                                                      |
| string        |                                                      |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_hook_logs

**Endpoint**

**GET** /api/2/hooks/:id/logs

**Resource Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **id**        | Set to the id of the Hook that you want to return.   |
|               |                                                      |
| required      |                                                      |
|               |                                                      |
| string        |                                                      |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

create_hook

**Endpoint**

**POST** /api/2/hooks

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **type**         | The type of hook function that will be created.   |
|                  | Must be one of:                                   |
| required         |                                                   |
|                  | - pre-authentication                              |
| string           |                                                   |
|                  | See the [Smart Hooks                              |
|                  | Overview](https://devel                           |
|                  | opers.onelogin.com/api-docs/2/hooks/overview) for |
|                  | more detail on the types of hooks available, the  |
|                  | context object inserted into the hook function,   |
|                  | and how to return from your hook function.        |
+------------------+---------------------------------------------------+
| **disabled**     | Default true. Indicates if function is available  |
|                  | for execution or not.                             |
| required         |                                                   |
|                  |                                                   |
| boolean          |                                                   |
+------------------+---------------------------------------------------+
| **timeout**      | Default 1. The number of seconds to allow the     |
|                  | hook function to run before before timing out.    |
| required         | Maximum timeout varies based on the type of       |
|                  | hook. [See overview for                           |
| integer          | more details](https://de                          |
|                  | velopers.onelogin.com/api-docs/2/hooks/overview). |
+------------------+---------------------------------------------------+
| **env_vars**     | An array of [predefined environment               |
|                  | variables](https://developers.onelogin.com        |
| required         | /api-docs/2/hooks/create-environment-variable) to |
|                  | be supplied to the function at runtime.           |
| array            |                                                   |
+------------------+---------------------------------------------------+
| **runtime**      | The Node.js version to execute the hook           |
|                  | function with.                                    |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **c              | Defaults to latest version of context. Supports   |
| ontext_version** | semantic versioning. e.g. \>= 1.0.0\"             |
|                  |                                                   |
| optional         | Each type of Smart Hook has its own set of        |
|                  | context versions. See the docs for the type of    |
| string           | hook you are creating to get the desired version. |
+------------------+---------------------------------------------------+
| **retries**      | Default 0. Max 4. Number of retries if            |
|                  | execution fails.                                  |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **options**      | A set of attributes allow control over the        |
|                  | information that is included in the hook context. |
| object           |                                                   |
|                  | See the docs for each hook type to learn about    |
|                  | which config options are available.               |
|                  |                                                   |
|                  | - [Pre-Authentication Hoo                         |
|                  | k](https://developers.onelogin.com/api-docs/2/sma |
|                  | rt-hooks/types/pre-authentication#config-options) |
|                  |                                                   |
|                  | [User-Migration                                   |
|                  |  Hook](https://developers.onelogin.com/api-docs/2 |
|                  | /smart-hooks/types/user-migration#config-options) |
+------------------+---------------------------------------------------+
| **packages**     | A list of public npm packages than will be        |
|                  | installed as part of the function build process.  |
| required         | Packages can be any version and support the       |
|                  | semantic versioning syntax used by NPM.           |
| object           |                                                   |
+------------------+---------------------------------------------------+
| **function**     | A base64 encoded string containing the javascript |
|                  | function code.                                    |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **conditions**   | An array of objects that let you limit the        |
|                  | execution of a hook to users in specific roles.   |
| array            |                                                   |
|                  | See the docs for each hook type to learn about    |
|                  | which config options are available.               |
|                  |                                                   |
|                  | [Pre-Authentication Hook                          |
|                  | ](https://developers.onelogin.com/api-docs/2/smar |
|                  | t-hooks/types/pre-authentication#hook-conditions) |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**

  409         Conflict                  **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

update_smart_hook

**Endpoint**

**PUT** /api/2/hooks/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Id**           | Set to the id of the hook that you want to        |
| (required)       | update.                                           |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+-------------------+--------------------------------------------------+
| Name/Type         | Description                                      |
+===================+==================================================+
| **type**          | The type of hook function that will be updated.  |
|                   | Must be one of:                                  |
| required          |                                                  |
|                   | - pre-authentication                             |
| integer           |                                                  |
|                   | See the [Smart Hooks                             |
|                   | Overview](https://developers.o                   |
|                   | nelogin.com/api-docs/2/smart-hooks/overview) for |
|                   | more detail on the types of hooks available, the |
|                   | context object inserted into the hook function,  |
|                   | and how to return from your hook function.       |
+-------------------+--------------------------------------------------+
| **disabled**      | Default true. Indicates if function is available |
|                   | for execution or not.                            |
| required          |                                                  |
|                   |                                                  |
| boolean           |                                                  |
+-------------------+--------------------------------------------------+
| **timeout**       | Default 1. The number of seconds to allow the    |
|                   | hook function to run before before timing out.   |
| required          | Maximum timeout varies based on the type of      |
|                   | hook. [See overview for                          |
| integer           | more details](https://dev                        |
|                   | elopers.onelogin.com/api-docs/2/hooks/overview). |
+-------------------+--------------------------------------------------+
| **env_vars**      | An array of [predefined environment              |
|                   | v                                                |
| required          | ariables](https://developers.onelogin.com/api-do |
|                   | cs/2/smart-hooks/create-environment-variable) to |
| array             | be supplied to the function at runtime.          |
+-------------------+--------------------------------------------------+
| **runtime**       | The Node.js version to execute the hook function |
|                   | with. Must be one of the following supported     |
| required          | versions:                                        |
|                   |                                                  |
| string            | nodejs18.x                                       |
+-------------------+--------------------------------------------------+
| **                | Defaults to latest version of context. Supports  |
| context_version** | semantic versioning. e.g. \>= 1.0.0\"            |
|                   |                                                  |
| optional          |                                                  |
|                   |                                                  |
| string            |                                                  |
+-------------------+--------------------------------------------------+
| **retries**       | Default 0. Max 4. Number of retries if           |
|                   | execution fails.                                 |
| required          |                                                  |
|                   |                                                  |
| integer           |                                                  |
+-------------------+--------------------------------------------------+
| **options**       | A set of attributes allow control over the       |
|                   | information that is included in the              |
| object            | hook context.                                    |
|                   |                                                  |
|                   | See the docs for each hook type to learn about   |
|                   | which config options are available.              |
|                   |                                                  |
|                   | - [Pre-Authentication Hook]                      |
|                   | (https://developers.onelogin.com/api-docs/2/smar |
|                   | t-hooks/types/pre-authentication#config-options) |
|                   |                                                  |
|                   | [User-Migration H                                |
|                   | ook](https://developers.onelogin.com/api-docs/2/ |
|                   | smart-hooks/types/user-migration#config-options) |
+-------------------+--------------------------------------------------+
| **packages**      | A list of public npm packages than will be       |
|                   | installed as part of the function build process. |
| required          | Packages can be any version and support the      |
|                   | semantic versioning syntax used by NPM.          |
| object            |                                                  |
+-------------------+--------------------------------------------------+
| **function**      | A base64 encoded string containing the           |
|                   | javascript function code.                        |
| required          |                                                  |
|                   |                                                  |
| string            |                                                  |
+-------------------+--------------------------------------------------+
| **conditions**    | An array of objects that let you limit the       |
|                   | execution of a hook to users in specific roles.  |
| array             |                                                  |
|                   | See the docs for each hook type to learn about   |
|                   | which config options are available.              |
|                   |                                                  |
|                   | [Pre-Authentication Hook](                       |
|                   | https://developers.onelogin.com/api-docs/2/smart |
|                   | -hooks/types/pre-authentication#hook-conditions) |
+-------------------+--------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_hook

**Endpoint**

**DEL** /api/2/hooks/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the hook that you want to        |
| (required)       | delete.                                           |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  202         Accepted                  **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

list_environment_variables

**Endpoint**

**GET** /api/2/hooks/envs

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_environment_variable

**Endpoint**

**GET** /api/2/hooks/envs/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the Hook Environment Variable    |
| (required)       | that you want to fetch.                           |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

create_environment_variable

**Endpoint**

**POST** /api/2/hooks/envs

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **name**         | The name for the environment variable that will   |
|                  | be used to retrieve the value from a hook         |
| required         | function.                                         |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **value**        | The secret value that will be encrypted at rest   |
|                  | and injected in applicable hook functions at      |
| required         | run time.                                         |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

update_environment_variable

**Endpoint**

**PUT** /api/2/hooks/envs/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the Hook Environment Variable    |
|                  | that you want to update.                          |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **value**        | The secret value that will be encrypted at rest   |
|                  | and injected in applicable hook functions at      |
| required         | run time.                                         |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_environment_variable

**Endpoint**

**DEL** /api/2/hooks/envs/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the Hook Environment Variable    |
|                  | that you want to update.                          |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

5.  **[User Mappings]{.underline}**

**Method**

list_mappings

**Endpoint**

**GET** /api/2/mappings

**Query Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+==============:+======================================================+
| **enabled**   | Defaults to true. When set to \`false\` will return  |
|               | all disabled mappings.                               |
| boolean       |                                                      |
+---------------+------------------------------------------------------+
| **ha          | Filters Mappings based on their Conditions. Values   |
| s_condition** | formatted as :, where name is the Condition to look  |
|               | for, and value is the value to find. Multiple        |
| string        | filters can be declared by using a comma delimited   |
|               | list. Wildcards are supported in both the name and   |
|               | value fields.                                        |
|               |                                                      |
|               | *For example:*                                       |
|               |                                                      |
|               | Single filter. has_condition=has_role:123456         |
|               |                                                      |
|               | Multiple                                             |
|               | filters. has_condition=has_role:123456,status:1      |
|               |                                                      |
|               | Wildcard for conditions. has_condition=\*:123456     |
|               |                                                      |
|               | Wildcard for condition                               |
|               | values. has_condition=has_role:\*                    |
+---------------+------------------------------------------------------+
| **has_con     | Filters Mappings based on their condition types.     |
| dition_type** |                                                      |
|               | Allowed values are:                                  |
| string        |                                                      |
|               | - **builtin** - actions that involve                 |
|               |   standard attributes                                |
|               |                                                      |
|               | - **custom** - actions that involve                  |
|               |   custom attributes                                  |
|               |                                                      |
|               | - **none** - no actions are defined                  |
|               |                                                      |
|               | *For example:*                                       |
|               |                                                      |
|               | Find Mappings using custom user                      |
|               | attributes has_condition_type=custom                 |
|               |                                                      |
|               | Find Mappings with no                                |
|               | conditions has_condition_type=none                   |
+---------------+------------------------------------------------------+
| *             | Filters Mappings based on their Actions. Values      |
| *has_action** | formatted as :, where name is the Action to look     |
|               | for, and value is the value to find. Multiple        |
| string        | filters can be declared by using a comma delimited   |
|               | list. Wildcards are supported in both the name and   |
|               | value fields.                                        |
|               |                                                      |
|               | *For example:*                                       |
|               |                                                      |
|               | Single filter. has_action=set_licenses:123456        |
|               |                                                      |
|               | Multiple                                             |
|               | f                                                    |
|               | ilters. has_action=set_groups:123456,set_usertype:\* |
|               |                                                      |
|               | Wildcard for actions. has_action=\*:123456           |
|               |                                                      |
|               | Wildcard for action                                  |
|               | values. has_action=set_userprincipalname:\*          |
+---------------+------------------------------------------------------+
| **has_        | Filters Mappings based on their action types.        |
| action_type** |                                                      |
|               | Allowed values are:                                  |
| string        |                                                      |
|               | - **builtin** - actions that involve                 |
|               |   standard attributes                                |
|               |                                                      |
|               | - **custom** - actions that involve                  |
|               |   custom attributes                                  |
|               |                                                      |
|               | - **none** - no actions are defined                  |
|               |                                                      |
|               | *For example:*                                       |
|               |                                                      |
|               | Find Mappings with no actions has_action_type=none   |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_mapping

**Endpoint**

**GET** /api/2/mappings/:id

**Resource Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **id**        | The id of the user mapping to locate.                |
|               |                                                      |
| required      |                                                      |
|               |                                                      |
| integer       |                                                      |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

Create_mapping

**Endpoint**

**POST** /api/2/mappings

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+=================:+===================================================+
| **name**         | The name of the mapping.                          |
|                  |                                                   |
| required         |                                                   |
+------------------+---------------------------------------------------+
| **enabled**      | Indicates if the mapping is enabled or not.       |
|                  |                                                   |
| required         |                                                   |
+------------------+---------------------------------------------------+
| **match**        | Indicates how conditions should be matched.       |
|                  |                                                   |
| required         | - **all** - Match all conditions                  |
|                  |                                                   |
|                  | **any** - Match any condition                     |
+------------------+---------------------------------------------------+
| **position**     | Indicates the order of the mapping. When \`null\` |
|                  | this will default to last position.               |
| required         |                                                   |
+------------------+---------------------------------------------------+
| **conditions**   | An array of conditions that the user must meet in |
|                  | order for the mapping to be applied.              |
| required         |                                                   |
|                  | - **source** - The source field to check.         |
|                  |   See [List                                       |
|                  |   Conditions](https://developers.onelogin.        |
|                  | com/api-docs/2/user-mappings/list-conditions) for |
|                  |   possible values.                                |
|                  |                                                   |
|                  | - **operator** - A valid operator for the         |
|                  |   selected condition source. See [List Condition  |
|                  |                                                   |
|                  |  Operators](https://developers.onelogin.com/api-d |
|                  | ocs/2/user-mappings/list-condition-operators) for |
|                  |   possible values.                                |
|                  |                                                   |
|                  | **value** - A plain text string or valid value    |
|                  | for the selected condition source. See [List      |
|                  | Condition                                         |
|                  | Values](https://developers.onelogin.com/ap        |
|                  | i-docs/2/user-mappings/list-condition-values) for |
|                  | possible values.                                  |
+------------------+---------------------------------------------------+
| **actions**      | An array of actions that will be applied to the   |
|                  | users that are matched by the conditions.         |
| required         |                                                   |
|                  | - **action** - The action to apply. See [List     |
|                  |   Actions](https://developers.onelogin.           |
|                  | com/api-docs/2/user-mappings/list-conditions) for |
|                  |   possible values.                                |
|                  |                                                   |
|                  | **value** - An array of strings. Items in the     |
|                  | array will be a plain text string or valid value  |
|                  | for the selected action. See [List Action         |
|                  | Values](https://developers.onelogin.com           |
|                  | /api-docs/2/user-mappings/list-action-values) for |
|                  | possible values. In most cases only a single item |
|                  | will be accepted in the array.                    |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

update_mapping

**Endpoint**

**PUT** /api/2/mappings/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Id**           | Set to the id of the mapping that you want to     |
| (required)       | update.                                           |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+----------------+---------------------------------------------+-------+
| Name/Type      | Description                                 | Type  |
+================+=============================================+=======+
| **name**       | The name of the mapping.                    | S     |
|                |                                             | tring |
| required       |                                             |       |
+----------------+---------------------------------------------+-------+
| **enabled**    | Indicates if the mapping is enabled or not. | Bo    |
|                |                                             | olean |
| required       |                                             |       |
+----------------+---------------------------------------------+-------+
| **match**      | Indicates how conditions should be matched. | S     |
|                |                                             | tring |
| required       | - **all** - Match all conditions            |       |
|                |                                             |       |
|                | **any** - Match any condition               |       |
+----------------+---------------------------------------------+-------+
| **position**   | Indicates the order of the mapping. When    | In    |
|                | \`null\` this will default to last          | teger |
| required       | position.                                   |       |
+----------------+---------------------------------------------+-------+
| **conditions** | An array of conditions that the user must   | Array |
|                | meet in order for the mapping to be         |       |
| required       | applied.                                    |       |
|                |                                             |       |
|                | - **source** - The source field to check.   |       |
|                |   See [List                                 |       |
|                |   Con                                       |       |
|                | ditions](https://developers.onelogin.com/ap |       |
|                | i-docs/2/user-mappings/list-conditions) for |       |
|                |   possible values.                          |       |
|                |                                             |       |
|                | - **operator** - A valid operator for the   |       |
|                |   selected condition source. See [List      |       |
|                |   Condition                                 |       |
|                |   Operators](                               |       |
|                | https://developers.onelogin.com/api-docs/2/ |       |
|                | user-mappings/list-condition-operators) for |       |
|                |   possible values.                          |       |
|                |                                             |       |
|                | **value** - A plain text string or valid    |       |
|                | value for the selected condition source.    |       |
|                | See [List Condition                         |       |
|                | Value                                       |       |
|                | s](https://developers.onelogin.com/api-docs |       |
|                | /2/user-mappings/list-condition-values) for |       |
|                | possible values.                            |       |
+----------------+---------------------------------------------+-------+
| **actions**    | An array of actions that will be applied to | Array |
|                | the users that are matched by the           |       |
| required       | conditions.                                 |       |
|                |                                             |       |
|                | - **action** - The action to apply.         |       |
|                |   See [List                                 |       |
|                |                                             |       |
|                | Actions](https://developers.onelogin.com/ap |       |
|                | i-docs/2/user-mappings/list-conditions) for |       |
|                |   possible values.                          |       |
|                |                                             |       |
|                | **value** - A plain text string or valid    |       |
|                | value for the selected action. See [List    |       |
|                | Action                                      |       |
|                | Va                                          |       |
|                | lues](https://developers.onelogin.com/api-d |       |
|                | ocs/2/user-mappings/list-action-values) for |       |
|                | possible values.                            |       |
+----------------+---------------------------------------------+-------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

dry_run_mapping

**Endpoint**

**POST** /api/2/mappings/:id/dryrun

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Id**           | Set to the id of the mapping that you want to     |
| (required)       | test.                                             |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_mapping

**Endpoint**

**DEL** /api/2/mappings/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the app that you want to update. |
| (required)       |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

list_conditions

**Endpoint**

**GET** /api/2/mappings/conditions

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

list_condition_operators

**Endpoint**

**GET** /api/2/mappings/conditions/:condition_value/operators

**Query Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **cond        | The value for the selected condition. For a complete |
| ition_value** | list of possible condition values see [List          |
|               | Conditons](https://developers.one                    |
| string        | login.com/api-docs/2/user-mappings/list-conditions). |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

list_condition_values

**Endpoint**

**GET** /api/2/mappings/conditions/:condition_value/values

**Query Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **cond        | The value for the selected condition. For a complete |
| ition_value** | list of possible condition values see [List          |
|               | Conditons](https://developers.one                    |
| string        | login.com/api-docs/2/user-mappings/list-conditions). |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

list_actions

**Endpoint**

**GET** /api/2/mappings/actions

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

list_action_values

**Endpoint**

**GET** /api/2/mappings/actions/:action_value/values

**Query Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **a           | The value for the selected action. For a complete    |
| ction_value** | list of possible action values see [List             |
|               | Actions](https://developers.                         |
| string        | onelogin.com/api-docs/2/user-mappings/list-actions). |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

bulk_sort_mappings

**Endpoint**

**PUT** /api/2/mappings/sort

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

6.  **[Users]{.underline}**

**Method**

get_users

**Endpoint**

**GET** /api/2/users

**Query Parameters**

+---------------------------+------------------------------------------+
| Name/Type                 | Description                              |
+===========================+==========================================+
| **created_since**         | An ISO8601 timestamp value that returns  |
|                           | all users created after a given date &   |
| string                    | time. e.g. 2020-07-01T20:38:24Z          |
+---------------------------+------------------------------------------+
| **created_until**         | An ISO8601 timestamp value that returns  |
|                           | all users created before a given date &  |
| string                    | time.                                    |
+---------------------------+------------------------------------------+
| **updated_since**         | An ISO8601 timestamp value that returns  |
|                           | all users updated after a given date &   |
| string                    | time.                                    |
+---------------------------+------------------------------------------+
| **updated_until**         | An ISO8601 timestamp value that returns  |
|                           | all users updated before a given date &  |
| string                    | time.                                    |
+---------------------------+------------------------------------------+
| **last_login_since**      | An ISO8601 timestamp value that returns  |
|                           | all users that logged in after a given   |
| string                    | date & time.                             |
+---------------------------+------------------------------------------+
| **last_login_until**      | An ISO8601 timestamp value that returns  |
|                           | all users that logged in before a given  |
| string                    | date & time.                             |
+---------------------------+------------------------------------------+
| **firstname**             | The first name of the user               |
|                           |                                          |
| string                    |                                          |
+---------------------------+------------------------------------------+
| **lastname**              | The last name of the user                |
|                           |                                          |
| string                    |                                          |
+---------------------------+------------------------------------------+
| **email**                 | The email address of the user            |
|                           |                                          |
| string                    |                                          |
+---------------------------+------------------------------------------+
| **username**              | The username for the user                |
|                           |                                          |
| string                    |                                          |
+---------------------------+------------------------------------------+
| **samaccountname**        | The AD login name for the user           |
|                           |                                          |
| string                    |                                          |
+---------------------------+------------------------------------------+
| **directory_id**          | The ID in OneLogin of the Directory that |
|                           | the user belongs to                      |
| string                    |                                          |
+---------------------------+------------------------------------------+
| **external_id**           | An external identifier that has be set   |
|                           | on the user                              |
| string                    |                                          |
+---------------------------+------------------------------------------+
| **app_id**                | The ID of a OneLogin Application         |
|                           |                                          |
| string                    |                                          |
+---------------------------+------------------------------------------+
| **user_ids**              | A comma separated list of OneLogin User  |
|                           | IDs                                      |
| string                    |                                          |
+---------------------------+------------------------------------------+
| **custom_attr             | The short name of a custom attribute.    |
| ibutes.{attribute_name}** | Note that the attribute name is prefixed |
|                           | with **custom_attributes.**              |
| string                    |                                          |
+---------------------------+------------------------------------------+
| **fields**                | A comma separated list user attributes   |
|                           | to return.                               |
| string                    |                                          |
|                           | e.g. i                                   |
|                           | d,firstname,lastname,profile_picture_url |
+---------------------------+------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

get_user_by_id

**Endpoint**

**GET** /api/2/users/:id

**Resource Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **id**        | Set to the id of the user that you want to return.   |
|               |                                                      |
| required      | If you don't know the user's id, use the [List       |
|               | Users](https://deve                                  |
| integer       | lopers.onelogin.com/api-docs/2/users/list-users) API |
|               | ca                                                   |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 
  -----------------------------------------------------------------------

**Method**

get_user_apps

**Endpoint**

**GET** /api/2/users/:id/apps

**Resource Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **id**        | Set to the id of the user that you want to return    |
|               | apps for.                                            |
| required      |                                                      |
|               |                                                      |
| integer       |                                                      |
+---------------+------------------------------------------------------+

**Query Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **ignore      | Defaults to \`false\`. When \`true\` will all apps   |
| _visibility** | that are assigned to a user regardless of their      |
|               | portal visibility setting.                           |
| boolean       |                                                      |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 
  -----------------------------------------------------------------------

**Method**

create_user

**Endpoint**

**POST** /api/2/users

**Request Parameters**

+----------------+-----+-----------------------------------------------+
| Name           | T   | Description                                   |
|                | ype |                                               |
+================+=====+===============================================+
| **username**   | str | A username for the user.                      |
|                | ing |                                               |
| required       |     |                                               |
+----------------+-----+-----------------------------------------------+
| **email**      | str | A valid email for the user.                   |
|                | ing |                                               |
| required       |     |                                               |
+----------------+-----+-----------------------------------------------+
| **firstname**  | str | The user's first name.                        |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **lastname**   | str | The user's last name.                         |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **password**   | str | The password to set for a user.               |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **password_    | str | Required if the password is being set.        |
| confirmation** | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **passwo       | str | Use this when importing a password that's     |
| rd_algorithm** | ing | already hashed.                               |
|                |     |                                               |
|                |     | **salt+sha256 or sha256+salt**\               |
|                |     | Set to the password value using a             |
|                |     | SHA-256-encoded value.\                       |
|                |     | If you include a salt value in your request,  |
|                |     | prepend the salt value to the cleartext       |
|                |     | password value before SHA-256-encoding it.\   |
|                |     | \                                             |
|                |     | *For example, if your salt value              |
|                |     | is **hello** and your cleartext password      |
|                |     | value is **password**, the value you need to  |
|                |     | SHA-256-encode is **hellopassword**. The      |
|                |     | resulting encoded value would be\             |
|                |     | 9fb8dc1cdabee85d13f5b                         |
|                |     | 4ba680a5e71cb8c80e78e5ffe8c01b698fa39346006.* |
|                |     |                                               |
|                |     | **b​crypt**\                                   |
|                |     | Set to the password value using a             |
|                |     | bcrypt-encoded value that produces a bcrypt   |
|                |     | hash with **\$2a** at the beginning. We       |
|                |     | currently only support bcrypt values that     |
|                |     | begin with **\$2a**.                          |
+----------------+-----+-----------------------------------------------+
| **salt**       | str | The salt value used with the                  |
|                | ing | password_algorithm.                           |
+----------------+-----+-----------------------------------------------+
| **title**      | str | The user's job title.                         |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **department** | str | The user's department.                        |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **company**    | str | The user's company.                           |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **comment**    | str | Free text related to the user.                |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **group_id**   | i   | The ID of the Group in OneLogin that the user |
|                | nte | will be assigned to.                          |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **role_ids**   | ar  | A list of OneLogin Role IDs the user will be  |
|                | ray | assigned to.                                  |
+----------------+-----+-----------------------------------------------+
| **phone**      | str | The [E.164                                    |
|                | ing | forma                                         |
|                |     | t](https://en.wikipedia.org/wiki/E.164) phone |
|                |     | number for a user.                            |
+----------------+-----+-----------------------------------------------+
| **state**      | i   | 0: Unapproved\                                |
|                | nte | 1: Approved\                                  |
|                | ger | 2: Rejected\                                  |
|                |     | 3: Unlicensed                                 |
+----------------+-----+-----------------------------------------------+
| **status**     | i   | 0: Unactivated\                               |
|                | nte | 1: Active\                                    |
|                | ger | 2: Suspended\                                 |
|                |     | 3: Locked\                                    |
|                |     | 4: Password expired\                          |
|                |     | 5: Awaiting password reset\                   |
|                |     | 7: Password Pending\                          |
|                |     | 8: Security questions required                |
+----------------+-----+-----------------------------------------------+
| **             | i   | The ID of the OneLogin Directory the user     |
| directory_id** | nte | will be assigned to.                          |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **tr           | i   | The ID of the OneLogin Trusted IDP the user   |
| usted_idp_id** | nte | will be assigned to.                          |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **m            | i   | The ID of the user's manager in Active        |
| anager_ad_id** | nte | Directory.                                    |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **man          | i   | The OneLogin User ID for the user's manager.  |
| ager_user_id** | nte |                                               |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **sa           | str | The user's Active Directory username.         |
| maccountname** | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **member_of**  | str | The user's directory membership.              |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **userp        | str | The principle name of the user.               |
| rincipalname** | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **distin       | str | The distinguished name of the user.           |
| guished_name** | ing |                                               |
+----------------+-----+-----------------------------------------------+
| *              | str | The ID of the user in an external directory.  |
| *external_id** | ing |                                               |
+----------------+-----+-----------------------------------------------+
| *              | str | The name configured for use in other          |
| *openid_name** | ing | applications that accept OpenID for sign-in.  |
+----------------+-----+-----------------------------------------------+
| **invalid_lo   | i   | The number of sequential invalid login        |
| gin_attempts** | nte | attempts the user has made.                   |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **preferred    | str | The 2-character language locale for the user, |
| _locale_code** | ing | such as en for English or es for Spanish.     |
+----------------+-----+-----------------------------------------------+
| **custo        | obj | An object to contain any other custom         |
| m_attributes** | ect | attributes you have configured.               |
+----------------+-----+-----------------------------------------------+

**Query Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **mappings**     | Controls how mappings will be applied to the user |
|                  | on creation.                                      |
|                  |                                                   |
|                  | Defaults to **async**.                            |
|                  |                                                   |
|                  | **async**: Mappings run after the API returns a   |
|                  | response\                                         |
|                  | **sync**: Mappings run before the API returns a   |
|                  | response\                                         |
|                  | **disabled**: Mappings don't run for this user.   |
+------------------+---------------------------------------------------+
| **v              | Will passwords validate against the User Policy?  |
| alidate_policy** |                                                   |
|                  | Defaults to **true**.                             |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

update_user

**Endpoint**

**PUT** /api/2/users/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the user that you want to        |
|                  | update.                                           |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Query Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **mappings**     | Controls how mappings will be applied to the user |
|                  | on update.                                        |
|                  |                                                   |
|                  | Defaults to **async**                             |
|                  |                                                   |
|                  | **async**: Mappings will be run after the API     |
|                  | returns a response\                               |
|                  | **sync**: Mappings will be run before the API     |
|                  | returns a response\                               |
|                  | **disabled**: Mappings will not be run for        |
|                  | this user                                         |
+------------------+---------------------------------------------------+
| **v              | Will passwords be validated against the User      |
| alidate_policy** | Policy?                                           |
|                  |                                                   |
|                  | Defaults to **true**                              |
+------------------+---------------------------------------------------+

**Request Parameters**

+----------------+-----+-----------------------------------------------+
| Name           | T   | Description                                   |
|                | ype |                                               |
+================+=====+===============================================+
| **username**   | str | A username for the user                       |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **email**      | str | A valid email for the user                    |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **firstname**  | str | The users first name                          |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **lastname**   | str | The users last name                           |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **password**   | str | The password to set for a user.               |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **password_    | str | Required if the password is being set         |
| confirmation** | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **passwo       | str | Use this when importing a password has        |
| rd_algorithm** | ing | already been hashed.                          |
|                |     |                                               |
|                |     | **salt+sha256 or sha256+salt**\               |
|                |     | Set to the password value using a             |
|                |     | SHA-256-encoded value.\                       |
|                |     | If you are including your own salt value in   |
|                |     | your request, prepend the salt value to the   |
|                |     | cleartext password value before               |
|                |     | SHA-256-encoding it.\                         |
|                |     | \                                             |
|                |     | *For example, if your salt value              |
|                |     | is **hello** and your cleartext password      |
|                |     | value is **password**, the value you need to  |
|                |     | SHA-256-encode is **hellopassword**. The      |
|                |     | resulting encoded value would be\             |
|                |     | 9fb8dc1cdabee85d13f5b                         |
|                |     | 4ba680a5e71cb8c80e78e5ffe8c01b698fa39346006.* |
|                |     |                                               |
|                |     | **b​crypt**\                                   |
|                |     | Set to the password value using a             |
|                |     | bcrypt-encoded value that produces a bcrypt   |
|                |     | hash with **\$2a** at the beginning. We       |
|                |     | currently only support bcrypt values that     |
|                |     | begin with **\$2a**.                          |
+----------------+-----+-----------------------------------------------+
| **salt**       | str | The salt value that was used with the         |
|                | ing | password_algorithm.                           |
+----------------+-----+-----------------------------------------------+
| **title**      | str | The users job title                           |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **department** | str | The users department                          |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **company**    | str | The company the user belongs to               |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **comment**    | str | Free text related to the user                 |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **group_id**   | i   | The ID of the Group in OneLogin that the user |
|                | nte | will be assigned to                           |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **role_ids**   | ar  | A list of OneLogin Role IDs the user will be  |
|                | ray | assigned to.                                  |
+----------------+-----+-----------------------------------------------+
| **phone**      | str | The [E.164                                    |
|                | ing | forma                                         |
|                |     | t](https://en.wikipedia.org/wiki/E.164) phone |
|                |     | number for a user.                            |
+----------------+-----+-----------------------------------------------+
| **state**      | i   | 0: Unapproved\                                |
|                | nte | 1: Approved\                                  |
|                | ger | 2: Rejected\                                  |
|                |     | 3: Unlicensed                                 |
+----------------+-----+-----------------------------------------------+
| **status**     | i   | 0: Unactivated\                               |
|                | nte | 1: Active\                                    |
|                | ger | 2: Suspended\                                 |
|                |     | 3: Locked\                                    |
|                |     | 4: Password expired\                          |
|                |     | 5: Awaiting password reset\                   |
|                |     | 7: Password Pending\                          |
|                |     | 8: Security questions required                |
+----------------+-----+-----------------------------------------------+
| **             | i   | The ID of the OneLogin Directory the user     |
| directory_id** | nte | will be assigned to                           |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **tr           | i   | The ID of the OneLogin Trusted IDP the user   |
| usted_idp_id** | nte | will be assigned to                           |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **m            | i   | The ID of the users manager in Active         |
| anager_ad_id** | nte | Directory                                     |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **man          | i   | The OneLogin User ID of the users manager     |
| ager_user_id** | nte |                                               |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **sa           | str | The users Active Directory username           |
| maccountname** | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **member_of**  | str | The users directory membership                |
|                | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **userp        | str | The principle name of the user                |
| rincipalname** | ing |                                               |
+----------------+-----+-----------------------------------------------+
| **distin       | str | The distinguished name of the user            |
| guished_name** | ing |                                               |
+----------------+-----+-----------------------------------------------+
| *              | str | The ID of the user in an external directory   |
| *external_id** | ing |                                               |
+----------------+-----+-----------------------------------------------+
| *              | str | The name configured for use in other          |
| *openid_name** | ing | applications that accept OpenID for sign-in.  |
+----------------+-----+-----------------------------------------------+
| **invalid_lo   | i   | The number of sequential invalid login        |
| gin_attempts** | nte | attempts the user has made.                   |
|                | ger |                                               |
+----------------+-----+-----------------------------------------------+
| **preferred    | str | The 2-character language locale for the user, |
| _locale_code** | ing | such as en for English or es for Spanish.     |
+----------------+-----+-----------------------------------------------+
| **custo        | obj | An object to contain any other custom         |
| m_attributes** | ect | attributes you have configured.               |
+----------------+-----+-----------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

delete_user

**Endpoint**

**DEL** /api/2/users/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the app that you want to delete. |
| (required)       |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_custom_attributes

**Endpoint**

**GET** /api/2/users/custom_attributes

**Resource Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **id**        | Set to the id of the user that you want to return    |
|               | apps for.                                            |
| required      |                                                      |
|               |                                                      |
| integer       |                                                      |
+---------------+------------------------------------------------------+

**Query Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **ignore      | Defaults to \`false\`. When \`true\` will all apps   |
| _visibility** | that are assigned to a user regardless of their      |
|               | portal visibility setting.                           |
| boolean       |                                                      |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

get_custom_attribute_by_id

**Endpoint**

**GET** /api/2/custom_attributes/:id

**Resource Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **id**        | Set to the id of the custom attribute that you want  |
|               | to return.                                           |
| required      |                                                      |
|               |                                                      |
| integer       |                                                      |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

create_custom_attributes

**Endpoint**

**POST** /api/2/custom_attributes

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Name**         | A descriptive name for the custom field.          |
|                  |                                                   |
| string           |                                                   |
|                  |                                                   |
| required         |                                                   |
+------------------+---------------------------------------------------+
| **Shortname**    | A unique identifier for the custom field.         |
|                  |                                                   |
| string           |                                                   |
|                  |                                                   |
| required         |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  400         Bad Request               

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

update_custom_attributes

**Endpoint**

**PUT** /api/2/custom_attributes/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Id**           | Set to the id of the custom attribute that you    |
| (required)       | want to update.                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Name**         | A descriptive name for the custom field.          |
|                  |                                                   |
| string           |                                                   |
|                  |                                                   |
| required         |                                                   |
+------------------+---------------------------------------------------+
| **Shortname**    | A unique identifier for the custom field.         |
|                  |                                                   |
| string           |                                                   |
|                  |                                                   |
| required         |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_custom_attributes

**Endpoint**

**DEL** /api/2/custom_attributes/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the custom attribute that you    |
| (required)       | want to delete.                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_user_roles

**Endpoint**

**GET** /api/1/users/:id/roles

**Resource Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **id**        | Set to the id of the user for which you want to      |
|               | return a list of assigned roles.                     |
| required      |                                                      |
|               |                                                      |
| integer       |                                                      |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  403         Forbidden                 **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

add_roles_for_user

**Endpoint**

**PUT** /api/1/users/:id/add_roles

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Id**           | Set to the id of the user to which you want to    |
| (required)       | assign a role.                                    |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Name**         | A descriptive name for the custom field.          |
|                  |                                                   |
| string           |                                                   |
|                  |                                                   |
| required         |                                                   |
+------------------+---------------------------------------------------+
| *                | Set to an array of one or more role IDs. The IDs  |
| *role_id_array** | must be positive integers.                        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| array of         |                                                   |
| integers         |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  403         Forbidden                 **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

remove_roles_for_user

**Endpoint**

**PUT** /api/1/users/:id/remove_roles

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Id**           | Set to the id of the user for whom you want to    |
| (required)       | remove a role.                                    |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| *                | Set to an array of one or more role IDs.          |
| *role_id_array** |                                                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| array of         |                                                   |
| integers         |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  403         Forbidden                 **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

update_password_insecure

**Endpoint**

**PUT** /api/1/users/set_password_clear_text/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Id**           | Set to the id of the user for which you want to   |
| (required)       | set or reset a password.                          |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **password**     | Set to the password value using cleartext.        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| **String**       |                                                   |
+------------------+---------------------------------------------------+
| **passwor        | Ensure that this value matches the password value |
| d_confirmation** | exactly.                                          |
|                  |                                                   |
|  required        |                                                   |
|                  |                                                   |
| **String**       |                                                   |
+------------------+---------------------------------------------------+
| **v              | Defaults to false. This will validate the         |
| alidate_policy** | password against the users OneLogin password      |
|                  | policy.                                           |
| optional         |                                                   |
|                  |                                                   |
| **Boolean**      |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  403         Forbidden                 **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

update_password_secure

**Endpoint**

**PUT** /api/1/users/set_password_using_salt/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Id**           | Set to the id of the user for which you want to   |
| (required)       | set or reset a password.                          |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+-----------------+----------------------------------------------------+
| Name/Type       | Description                                        |
+=================+====================================================+
| **password**    | Set to the password value using a SHA-256-encoded  |
|                 | value                                              |
| required        |                                                    |
|                 | .                                                  |
| string          |                                                    |
|                 | Note that the alpha characters in this has must    |
|                 | all be lower case.                                 |
+-----------------+----------------------------------------------------+
| **password      | This value must match the password value.          |
| _confirmation** |                                                    |
|                 |                                                    |
| required        |                                                    |
|                 |                                                    |
| string          |                                                    |
+-----------------+----------------------------------------------------+
| **passw         | Set to salt+sha256.                                |
| ord_algorithm** |                                                    |
|                 |                                                    |
| required        |                                                    |
|                 |                                                    |
| string          |                                                    |
+-----------------+----------------------------------------------------+
| **              | Optional. If your password hash has been salted    |
| password_salt** | then you can provide the salt used in this param.  |
|                 |                                                    |
| string          | This assumes that the salt was prepended to the    |
|                 | password before doing the SHA256 hash.             |
|                 |                                                    |
|                 | The API supports a salt value that is up to 40     |
|                 | characters long.                                   |
+-----------------+----------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  403         Forbidden                 **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

set_custom_attributes

**Endpoint**

**PUT** /api/1/users/:id/set_custom_attributes

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Id**           | Set to the id of the user for whom you want to    |
| (required)       | set custom attribute values.                      |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **cus            | Provide one or more key value pairs composed of   |
| tom_attributes** | the custom attribute field shortname and the      |
|                  | value that you want to set the field to. See      |
| required         | the **Sample Request Body** below for the         |
|                  | required format.                                  |
| object           |                                                   |
|                  | Any existing custom attribute field value will    |
|                  | be overwritten.                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  403         Forbidden                 **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

set_user_state

**Endpoint**

**PUT** /api/1/users/:id/set_state

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **Id**           | Set to the id of the user whose state you want to |
| (required)       | update.                                           |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **state**        | Set to the state value. Valid values include:     |
|                  |                                                   |
| required         | Unapproved: 0\                                    |
|                  | Approved (licensed): 1\                           |
| integer          | Rejected: 2\                                      |
|                  | Unlicensed: 3                                     |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  403         Forbidden                 **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

log_out_user

**Endpoint**

**PUT** /api/1/users/:id/logout

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the user that you want to log    |
|                  | out.                                              |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  403         Forbidden                 **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

lock_user_account

**Endpoint**

**PUT** /api/1/users/:id/lock_user

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the user whose account you want  |
|                  | to lock.                                          |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **locked_until** | Set to the number of minutes for which you want   |
|                  | to lock the user account.                         |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  403         Forbidden                 **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

7.  **[Events]{.underline}**

**Method**

list_events

**Endpoint**

**GET** /api/1/events

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_event_types

**Endpoint**

**GET** /api/1/events/types

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  -----------------------------------------------------------------------

**Method**

get_events

**Endpoint**

**GET** /api/1/events/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the event that you want to       |
|                  | return.                                           |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

8.  **[Groups]{.underline}**

**Method**

get_groups

**Endpoint**

**GET** /api/1/groups

**Query Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the group that you want to       |
|                  | return.                                           |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_group_by_id

**Endpoint**

**GET** /api/1/groups/:id

**Query Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the group that you want to       |
|                  | return.                                           |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

9.  **[Invite Links]{.underline}**

**Method**

generate_invite_link

**Endpoint**

**POST** /api/1/invites/get_invite_link

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **email**        | Set to the user email address to generate an      |
|                  | invite link.                                      |
| required         |                                                   |
|                  | The value is case sensitive.                      |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

send_invite_link

**Endpoint**

**POST** /api/1/invites/send_invite_link

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **email**        | Set to the user email address to generate an      |
|                  | invite link.                                      |
| required         |                                                   |
|                  | The value is case sensitive.                      |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **               | To send an invite email to a different address    |
| personal_email** | than the one provided in email, provide it here.  |
|                  | The invite link is sent to this address instead.  |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

10. **[Login Pages]{.underline}**

**Method**

create_session_login_token

**Endpoint**

**POST** /api/1/login/auth

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **use            | Set to the username or email of the user that you |
| rname_or_email** | want to log in.                                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **password**     | Set to the password of the user that you want to  |
|                  | log in.                                           |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **subdomain**    | Set to the subdomain of the user that you want to |
|                  | log in.                                           |
| required         |                                                   |
|                  | For example, if your OneLogin URL                 |
| string           | is splinkly.onelogin.com, enter splinkly as the   |
|                  | subdomain value.                                  |
|                  |                                                   |
|                  | **Custom Domains**                                |
|                  |                                                   |
|                  | When a custom domain is in use you still need to  |
|                  | provide your original OneLogin subdomain in this  |
|                  | field. Do not use the custom domain here.         |
+------------------+---------------------------------------------------+
| **fields**       | Optional. A comma separated list of user fields   |
|                  | to return.                                        |
| string           |                                                   |
|                  | If this attribute is ommited then by default the  |
|                  | users id, firstname, lastname, email, and         |
|                  | username will be returned.                        |
|                  |                                                   |
|                  | Otherwise only the list of fields supplied will   |
|                  | be returned. For a full list of possible user     |
|                  | fields                                            |
|                  | see [user resource](https://develop               |
|                  | ers.onelogin.com/api-docs/1/users/user-resource). |
|                  |                                                   |
|                  | To return custom attributes prefix the field      |
|                  | with \`custom_attributes\`.                       |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

verify_factor

**Endpoint**

**POST** /api/1/login/verify_factor

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **device_id**    | Provide the MFA device_id you are submitting for  |
|                  | verification. The device_id is supplied by        |
| required         | the [Create Session Login                         |
|                  | Token](https://developers.onelogin.com/api-       |
| string           | docs/1/login-page/create-session-login-token) API |
+------------------+---------------------------------------------------+
| **state_token**  | Provide the state_token associated with the       |
|                  | MFA device_id you are submitting for              |
| required         | verification. The state_token is supplied by      |
|                  | the [Create Session Login                         |
| string           | Token](https://developers.onelogin.com/api-d      |
|                  | ocs/1/login-page/create-session-login-token) API. |
+------------------+---------------------------------------------------+
| **otp_token**    | Provide the OTP value for the MFA factor you are  |
|                  | submitting for verification.                      |
| string           |                                                   |
|                  | For some MFA factors; such as OneLogin OTP SMS,   |
|                  | which requires the user to request an OTP;        |
|                  | the otp_token value is not required, and if not   |
|                  | included, returns a 200 OK - Pending result.      |
|                  | You'll make a subsequent Verify Factor API call   |
|                  | to provide the otp_token value once it has been   |
|                  | provided to the user.                             |
|                  |                                                   |
|                  | In the case of other MFA factors; such as Google  |
|                  | Authenticator or Yubikey, which immediately       |
|                  | provide an OTP value to the user;                 |
|                  | the otp_token value is required as it is          |
|                  | immediately available to the user                 |
+------------------+---------------------------------------------------+
| *                | When verifying MFA via Protect Push, set this     |
| *do_not_notify** | to true to stop additional push notifications     |
|                  | being sent to the OneLogin Protect device.        |
| boolean          |                                                   |
|                  | e.g. You would make the first request with this   |
|                  | set to false to trigger a notification on the     |
|                  | users device requesting MFA. You would then make  |
|                  | subsequent requests to this API with this value   |
|                  | set to true while polling for the users           |
|                  | approve/deny response.                            |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  400         Bad Request               

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

11. **[Multi Factor Authentication]{.underline}**

**Method**

get_available_mfa_factors

**Endpoint**

**GET** /api/2/mfa/users/\<user_id\>/factors

**Resource Parameter**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set to the id of the user.                        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

enroll_mfa_factor

**Endpoint**

**POST** /api/2/mfa/users/\<user_id\>/registrations

**Resource Parameter**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set to the id of the user.                        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **factor_id**    | The identifier of the factor to enroll the user   |
|                  | with.                                             |
| required         |                                                   |
|                  | See [Get Available                                |
| integer          | Fact                                              |
|                  | ors](https://developers.onelogin.com/api-docs/2/m |
|                  | ulti-factor-authentication/available-factors) for |
|                  | a list of possible id values.                     |
+------------------+---------------------------------------------------+
| **display_name** | A name for the users device                       |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **expires_in**   | Defaults to 120. Valid values are: 120-900        |
|                  | seconds.                                          |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **verified**     | Defaults to false. The following factors support  |
|                  | verified = true as part of the initial            |
| boolean          | registration request: OneLogin SMS, OneLogin      |
|                  | Voice, OneLogin Email. When using verified        |
|                  | = true it is critical that the supported factors  |
|                  | have pre-verified values, most likely imported    |
|                  | from an existing directory or by the users        |
|                  | themselvdes.                                      |
|                  |                                                   |
|                  | Factors such as Authenticator and OneLogin        |
|                  | Protect do not support verification = true as the |
|                  | user interaction is required to verify            |
|                  | the factor.                                       |
+------------------+---------------------------------------------------+
| **redirect_to**  | Optional. Only applies to Email MagicLink factor. |
|                  |                                                   |
| string           | Redirects MagicLink success page to specified URL |
|                  | after 2 seconds. Email must already be configured |
|                  | by the user.                                      |
+------------------+---------------------------------------------------+
| **               | Optional. Only applies to SMS factor.             |
| custom_message** |                                                   |
|                  | A message template that will be sent via SMS. Max |
| string           | length of the message after template items are    |
|                  | inserted is 160 characters including the OTP      |
|                  | code. SMS must already be configured by the user. |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_enrolled_factor

**Endpoint**

**GET** /api/2/mfa/users/\<user_id\>/devices

**Resource Parameter**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set to the id of the user.                        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

Remove_mfa_factor

**Endpoint**

**DEL** /api/2/mfa/users/\<user_id\>/devices/\<device_id\>

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set to the id of the user.                        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **device_id**    | Set to the device_id of the MFA device.           |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  -----------------------------------------------------------------------

**Method**

activate_mfa_factor

**Endpoint**

**POST** /api/2/mfa/users/\<user_id\>/verifications

**Resource Parameter**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set to the id of the user.                        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **device_id**    | Required. Specifies the [factor to                |
|                  | be v                                              |
| integer          | erified](https://developers.onelogin.com/api-docs |
|                  | /2/multi-factor-authentication/enrolled-factors). |
+------------------+---------------------------------------------------+
| **expires_in**   | Optional. Sets the window of time in seconds that |
|                  | the [factor must be                               |
| integer          | verif                                             |
|                  | ied within](https://developers.onelogin.com/api-d |
|                  | ocs/2/multi-factor-authentication/verify-factor). |
|                  |                                                   |
|                  | Defaults to 120 seconds (2 minutes). Max 900      |
|                  | seconds (15 minutes).                             |
+------------------+---------------------------------------------------+
| **redirect_to**  | Optional. Only applies to Email MagicLink factor. |
|                  |                                                   |
| string           | Redirects MagicLink success page to specified URL |
|                  | after 2 seconds.                                  |
+------------------+---------------------------------------------------+
| **               | Optional. Only applies to SMS factor.             |
| custom_message** |                                                   |
|                  | A message template that will be sent via SMS. Max |
| string           | length of the message after template items are    |
|                  | inserted is 160 characters including the          |
|                  | OTP code.                                         |
|                  |                                                   |
|                  | The following template variables can be included  |
|                  | in the message.                                   |
|                  |                                                   |
|                  | - {{otp_code}} - The security code.               |
|                  |                                                   |
|                  | {{otp_expiry}} - The number of minutes until the  |
|                  | one time code expires.                            |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

generate_mfa_token

**Endpoint**

**POST** /api/2/mfa/users/:user_id/mfa_token

**Resource Parameter**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set to the id of the user to generate a token.    |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **expires_in**   | Set the duration of the token in seconds.         |
|                  |                                                   |
| integer          | Token expiration defaults to 259200 seconds = 72  |
|                  | hours. 72 hours is the max value.                 |
+------------------+---------------------------------------------------+
| **reusable**     | Defines if the token is reusable multiple times   |
|                  | within the expiry window.                         |
| string           |                                                   |
|                  | Value defaults to false. If set to true, token    |
|                  | can be used multiple times, until it expires.     |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

verify_mfa_enrollment

**Endpoint**

**PUT** /api/2/mfa/users/\<user_id\>/registrations/\<registration_id\>

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set to the id of the user.                        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **r              | Set to the uuid of the registration.              |
| egistration_id** |                                                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| uuid             |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

verify_mfa_enrollment_get

**Endpoint**

**GET** /api/2/mfa/users/\<user_id\>/registrations/\<registration_id\>

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set to the id of the user.                        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **v              | The verification_id is returned on activation of  |
| erification_id** | the factor or you can get the device_id using     |
|                  | the [Activate                                     |
| integer          | F                                                 |
|                  | actor](https://developers.onelogin.com/api-docs/2 |
|                  | /multi-factor-authentication/activate-factor) API |
|                  | call.                                             |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

verify_auth_factor

**Endpoint**

**PUT** /api/2/mfa/users/\<user_id\>/verifications/\<verification_id\>

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set to the id of the user.                        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **v              | Set to the uuid of the registration.              |
| erification_id** |                                                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| uuid             |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **otp**          | OTP code provided by the device or SMS message    |
|                  | sent to user.                                     |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **device_id**    | ID of the specified device which has been         |
|                  | registered for the given user. Available on [Get  |
| integer          | Devi                                              |
|                  | ces](https://developers.onelogin.com/api-docs/2/m |
|                  | ulti-factor-authentication/available-factors) API |
|                  | call.                                             |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  403         Forbidden                 **-**
  -----------------------------------------------------------------------

**Method**

verify_auth_factor_get

**Endpoint**

**GET** /api/2/mfa/users/\<user_id\>/verifications/\<verification_id\>

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set to the id of the user.                        |
|                  |                                                   |
| required         | If you don't know the user's id, use the [Get     |
|                  | Users](https://devel                              |
| integer          | opers.onelogin.com/api-docs/2/users/get-user) API |
|                  | call to return all users and their id values.     |
+------------------+---------------------------------------------------+
| **v              | The verification_id is returned on activation of  |
| erification_id** | the factor or you can get the device_id using     |
|                  | the [Activate                                     |
| required         | Factor]                                           |
|                  | (https://developers.onelogin.com/api-docs/2/multi |
| integer          | -factor-authentication/activate-factor) API call. |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

12. **[Connectors]{.underline}**

**Method**

list_connectors

**Endpoint**

**GET** /api/2/connectors

**Query Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **name**         | The name or partial name of the app to search     |
|                  | for.                                              |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **auth_method**  | Returns all connectors of a given type.           |
|                  |                                                   |
| number           | - 0 - Password                                    |
|                  |                                                   |
|                  | - 1 - OpenId                                      |
|                  |                                                   |
|                  | - 2 - SAML                                        |
|                  |                                                   |
|                  | - 3 - API                                         |
|                  |                                                   |
|                  | - 4 - Google                                      |
|                  |                                                   |
|                  | - 6 - Forms Based App                             |
|                  |                                                   |
|                  | - 7 - WSFED                                       |
|                  |                                                   |
|                  | - 8 -OpenId Connect                               |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

13. **[Privileges]{.underline}**

**Method**

list_privileges

**Endpoint**

**GET** /api/1/privileges

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

Create_privilege

**Endpoint**

**POST** api/1/privileges

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **name**         | The name of this privilege                        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **description**  | The description for this privilege                |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **privilege**    | An object containing statements that describe the |
|                  | level of access granted by this privilege.        |
| required         |                                                   |
|                  |                                                   |
| object           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  400         Bad Request               

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

update_privilege

**Endpoint**

**PUT** /api/1/privileges/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the privilege you want           |
|                  | to update.                                        |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **name**         | The name of this privilege                        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **description**  | The description for this privilege                |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **privilege**    | An object containing statements that describe the |
|                  | level of access granted by this privilege.        |
| required         |                                                   |
|                  |                                                   |
| object           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  403         Forbidden                 **-**
  -----------------------------------------------------------------------

**Method**

get_privilege

**Endpoint**

**GET** /api/1/privileges/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the privilege you want           |
|                  | to retrieve.                                      |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

delete_privilege

**Endpoint**

**DEL** /api/1/privileges/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the privilege you want           |
|                  | to delete.                                        |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_privilege_roles

**Endpoint**

**GET** /api/1/privileges/:id/roles

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the privilege.                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

add_privilege_to_roles

**Endpoint**

**POST** /api/1/privileges/:id/roles

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the privilege.                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **roles**        | Array of role_ids to which the privilege will be  |
|                  | assigned                                          |
| required         |                                                   |
|                  |                                                   |
| array            |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  400         Bad Request               

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

remove_role_from_privilege

**Endpoint**

**DEL** /api/1/privileges/:id/roles/:role_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the privilege.                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **role_id**      | Set to the id of the role you want to remove.     |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_privilege_users

**Endpoint**

**GET** /api/1/privileges/:id/users

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the privilege.                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

add_privilege_to_users

**Endpoint**

**POST** /api/1/privileges/:id/users

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the privilege.                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **users**        | Array of user_ids to which the privilege will be  |
|                  | assigned                                          |
| required         |                                                   |
|                  |                                                   |
| array            |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

remove_user_from_privilege

**Endpoint**

**DEL** /api/1/privileges/:id/users/:user_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the privilege.                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **user_id**      | Set to the id of the user you want to remove.     |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 
  -----------------------------------------------------------------------

14. **[Vigilance AI]{.underline}**

**Method**

track_event

**Endpoint**

**POST** /api/2/risk/events

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **verb**         | Verbs are used to distinguish between different   |
|                  | types of events.                                  |
| required         |                                                   |
|                  | Where possible use one of the following verbs to  |
| string           | describe the event. Alternately you can create    |
|                  | custom verbs to describe other types of actions   |
|                  | within your application.                          |
|                  |                                                   |
|                  | - **log-in** - A user successfully logged into    |
|                  |   your app                                        |
|                  |                                                   |
|                  | - **log-out** - The user has logged out           |
|                  |                                                   |
|                  | - **log-in-denied** - The user failed             |
|                  |   to authenticate                                 |
|                  |                                                   |
|                  | - **authentication-challenge** - Authentication   |
|                  |   was challenged (e.g. MFA was required)          |
|                  |                                                   |
|                  | - **authentication-challenge-pass** - The         |
|                  |   authentication challenge was passed             |
|                  |                                                   |
|                  | **authentication-challenge-fail** - The           |
|                  | authentication challenge failed                   |
+------------------+---------------------------------------------------+
| **ip**           | The IP address of the User's request.             |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **user_agent**   | The user agent of the User's request.             |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **user**         | An Object containing User details.                |
|                  |                                                   |
| required         | The available object parameters are:              |
|                  |                                                   |
| object           | - **id** - required A unique identifier for       |
|                  |   the user.                                       |
|                  |                                                   |
|                  | - **name** - A name for the user.                 |
|                  |                                                   |
|                  | - **authenticated** - A boolean value which       |
|                  |   indicates if the metadata supplied in this      |
|                  |   event should be considered as trusted for the   |
|                  |   user. Defaults to false.                        |
|                  |                                                   |
|                  | When using this API to track additional events    |
|                  | for the OneLogin Adaptive Authentication service  |
|                  | the user **id** must be in the following format.  |
|                  |                                                   |
|                  | {instance region}\_{OneLogin User Id}             |
|                  |                                                   |
|                  | E.g. US_12345678                                  |
+------------------+---------------------------------------------------+
| **source**       | This field can used for targeting custom rules    |
|                  | based on a group of people, customers, accounts,  |
| object           | or even a single user.                            |
|                  |                                                   |
|                  | The available object parameters are:              |
|                  |                                                   |
|                  | - **id** - A unique id that represents the source |
|                  |   of the event.                                   |
|                  |                                                   |
|                  | **name** - The name of the source                 |
+------------------+---------------------------------------------------+
| **session**      | A dictionary of extra information that provides   |
|                  | useful context about the session, for example the |
| object           | session ID, or some cookie information.           |
|                  |                                                   |
|                  | The available object parameters are:              |
|                  |                                                   |
|                  | **id** - If you use a database to track sessions, |
|                  | you can send us the session ID.                   |
+------------------+---------------------------------------------------+
| **device**       | Information about the device being used.          |
|                  |                                                   |
| object           | The available object parameters are:              |
|                  |                                                   |
|                  | **id** - This device's unique identifier          |
+------------------+---------------------------------------------------+
| **fp**           | Set to the value of the **\_\_tdli_fp** cookie.   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **published**    | Date and time of the event in IS08601 format.     |
|                  | Useful for preloading old events.                 |
| string           |                                                   |
|                  | Defaults to date time this API request            |
|                  | is received.                                      |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_risk_score

**Endpoint**

**POST** /api/2/risk/verify

**Request Parameters**

+------------+---------------------------------------------------------+
| Name/Type  | Description                                             |
+============+=========================================================+
| **ip**     | The IP address of the User's request.                   |
|            |                                                         |
| required   |                                                         |
|            |                                                         |
| string     |                                                         |
+------------+---------------------------------------------------------+
| **us       | The user agent of the User's request.                   |
| er_agent** |                                                         |
|            |                                                         |
| required   |                                                         |
|            |                                                         |
| string     |                                                         |
+------------+---------------------------------------------------------+
| **user**   | An Object containing User details.                      |
|            |                                                         |
| required   | The available object parameters are:                    |
|            |                                                         |
| object     | - **id** - required A unique identifier for the user.   |
|            |                                                         |
|            | - **name** - A name for the user.                       |
|            |                                                         |
|            | - **authenticated** - A boolean value which indicates   |
|            |   if the metadata supplied in this event should be      |
|            |   considered as trusted for the user. Defaults          |
|            |   to false.                                             |
|            |                                                         |
|            | When using this API to get a risk score for user of the |
|            | Adaptive Authentication service the user **id** must be |
|            | in the following format.                                |
|            |                                                         |
|            | {instance region}\_{OneLogin User Id}                   |
|            |                                                         |
|            | E.g. US_12345678                                        |
+------------+---------------------------------------------------------+
| **source** | This field can used for targeting custom rules based on |
|            | a group of people, customers, accounts, or even a       |
| object     | single user.                                            |
|            |                                                         |
|            | The available object parameters are:                    |
|            |                                                         |
|            | - **id** - A unique id that represents the source of    |
|            |   the event.                                            |
|            |                                                         |
|            | - **name** - The name of the source                     |
+------------+---------------------------------------------------------+
| *          | A dictionary of extra information that provides useful  |
| *session** | context about the session, for example the session ID,  |
|            | or some cookie information.                             |
| object     |                                                         |
|            | The available object parameters are:                    |
|            |                                                         |
|            | - **id** - If you use a database to track sessions, you |
|            |   can send us the session ID.                           |
+------------+---------------------------------------------------------+
| **device** | Information about the device being used.                |
|            |                                                         |
| object     | The available object parameters are:                    |
|            |                                                         |
|            | - **id** - This device's unique identifier              |
+------------+---------------------------------------------------------+
| **fp**     | Set to the value of the **\_\_tdli_fp** cookie.         |
|            |                                                         |
| string     |                                                         |
+------------+---------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

create_rule

**Endpoint**

**POST** /api/2/risk/rules

**Request Parameters**

+---------+------------------------------------------------------------+
| Na      | Description                                                |
| me/Type |                                                            |
+=========+============================================================+
| *       | The name of this rule                                      |
| *name** |                                                            |
|         |                                                            |
| r       |                                                            |
| equired |                                                            |
|         |                                                            |
| string  |                                                            |
+---------+------------------------------------------------------------+
| *       | The type parameter specifies the type of rule that will be |
| *type** | created. Currently the following types are supported:      |
|         |                                                            |
| r       | - **blacklist** - If an event contains a value in a        |
| equired |   blocklist the risk score will always be 100 (HIGH).      |
|         |                                                            |
| string  | **whitelist** - An allowed list value will override a      |
|         | blocklist with the same target parameter.                  |
+---------+------------------------------------------------------------+
| **t     | The target parameter that will be used when evaluating the |
| arget** | rule against an incoming event. Currently the following    |
|         | targets are supported:                                     |
| string  |                                                            |
|         | - **location.ip**                                          |
| r       |                                                            |
| equired | **location.address.country_iso_code**                      |
+---------+------------------------------------------------------------+
| **fi    | An array of string values to evaluate against each event.  |
| lters** | It could be a list of IP addresses or country code         |
|         | or name.                                                   |
| string  |                                                            |
|         |                                                            |
| r       |                                                            |
| equired |                                                            |
+---------+------------------------------------------------------------+
| **s     | The source can be used to scope rules to a specific group  |
| ource** | of people, customers, or even a single user. It matches    |
|         | against the source.id parameter that you can send with a   |
| string  | Event or Verify API request.                               |
+---------+------------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

list_rules

**Endpoint**

**GET** /api/2/risk/rules

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_rule

**Endpoint**

**GET** /api/2/risk/rules/\<id\>

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The ID of the rule to retrieve.                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

update_rule

**Endpoint**

**PUT** /api/2/risk/rules/\<id\>

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The ID of the rule to update.                     |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The ID of the rule to update.                     |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

delete_rule

**Endpoint**

**DEL** /api/2/risk/rules/\<id\>

**Request Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The ID of the rule to retrieve.                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_score_summary

**Endpoint**

**GET** /api/2/risk/scores

**Query Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **before**       | Optional ISO8601 formatted date string. Defaults  |
|                  | to current date. Maximum date is 90 days ago.     |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

15. **[SAML Assertions]{.underline}**

**Method**

generate_saml_assertion

**Endpoint**

**POST** /api/2/saml_assertion

**Request Parameters**

+---------------+------------------------------------------------------+
| Name/Type     | Description                                          |
+===============+======================================================+
| **userna      | Set this to the username or email of the OneLogin    |
| me_or_email** | user accessing the app for which you want to         |
|               | generate a SAML token.                               |
| required      |                                                      |
|               |                                                      |
| string        |                                                      |
+---------------+------------------------------------------------------+
| **password**  | Password of the OneLogin user accessing the app for  |
|               | which you want to generate a SAML token.             |
| required      |                                                      |
|               |                                                      |
| string        |                                                      |
+---------------+------------------------------------------------------+
| **app_id**    | App ID of the app for which you want to generate a   |
|               | SAML token. This is the app ID in OneLogin.          |
| required      |                                                      |
|               |                                                      |
| string        |                                                      |
+---------------+------------------------------------------------------+
| **subdomain** | Set to the subdomain of the OneLogin user accessing  |
|               | the app for which you want to generate a SAML token. |
| required      |                                                      |
|               | For example, if your OneLogin URL                    |
| string        | is splinkly.onelogin.com, enter splinkly as the      |
|               | subdomain value.                                     |
+---------------+------------------------------------------------------+
| *             | If you are using this API in a scenario in which MFA |
| *ip_address** | is required and you'll need to be able to honor IP   |
|               | address allow-listing defined in MFA policies,       |
| string        | provide this parameter and set its value to the      |
|               | allowed IP address that needs to be bypassed.        |
|               |                                                      |
|               | By making this a parameter that the developer passes |
|               | in, the API enables you to tailor it to your use     |
|               | case. For example:                                   |
|               |                                                      |
|               | - You are building a web app and, in this case, only |
|               |   the web app knows the IP address of the user       |
|               |   accessing the application. This is the IP address  |
|               |   that you should pass in the parameter to determine |
|               |   if MFA is required or should be bypassed.          |
|               |                                                      |
|               | You are building a native app and, in this case,     |
|               | only the native app knows the IP address of the      |
|               | machine the request is being made from. This is the  |
|               | IP address that you should pass in the parameter to  |
|               | determine if MFA is required or should be bypassed.  |
+---------------+------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

verify_factor_saml

**Endpoint**

**POST** /api/2/saml_assertion/verify_factor

**Request Parameters**

+-----------+----------------------------------------------------------+
| Name/Type | Description                                              |
+===========+==========================================================+
| *         | App ID of the app for which you want to generate a SAML  |
| *app_id** | token. This is the app ID in OneLogin.                   |
|           |                                                          |
| required  |                                                          |
|           |                                                          |
| string    |                                                          |
+-----------+----------------------------------------------------------+
| **de      | Provide the MFA device_id you are submitting for         |
| vice_id** | verification. The device_id is supplied by the [Generate |
|           | SAML                                                     |
| required  | Assertion](https://developers.onelogin.com/              |
|           | api-docs/2/saml-assertions/generate-saml-assertion) API. |
| string    |                                                          |
+-----------+----------------------------------------------------------+
| **stat    | Provide the state_token associated with the              |
| e_token** | MFA device_id you are submitting for verification.       |
|           | The state_token is supplied by the [Generate SAML        |
| required  | Assertion](https://developers.onelogin.com/              |
|           | api-docs/2/saml-assertions/generate-saml-assertion) API. |
| string    |                                                          |
+-----------+----------------------------------------------------------+
| **ot      | Provide the OTP value for the MFA factor you are         |
| p_token** | submitting for verification.                             |
|           |                                                          |
| string    | For some MFA factors; such as OneLogin OTP SMS, which    |
|           | requires the user to request an OTP; the otp_token value |
|           | is not required, and if not included, returns a 200 OK - |
|           | Pending result. You'll make a subsequent Verify Factor   |
|           | API call to provide the otp_token value once it has been |
|           | provided to the user.                                    |
|           |                                                          |
|           | In the case of other MFA factors; such as Google         |
|           | Authenticator or Yubikey, which immediately provide an   |
|           | OTP value to the user; the otp_token value is required   |
|           | as it is immediately available to the user.              |
+-----------+----------------------------------------------------------+
| **do_not  | When verifying MFA via Protect Push, set this to true to |
| _notify** | stop additional push notifications being sent to the     |
|           | OneLogin Protect device.                                 |
| boolean   |                                                          |
|           | e.g. You would make the first request with this set      |
|           | to false to trigger a notification on the users device   |
|           | requesting MFA. You would then make subsequent requests  |
|           | to this API with this value set to true while polling    |
|           | for the users approve/deny response.                     |
+-----------+----------------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

16. **[API Authorization]{.underline}**

**Method**

get_auth_servers

**Endpoint**

**GET** /api/2/api_authorizations

**Query Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **name**         | The name or partial name of the authorization     |
|                  | server to locate.                                 |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**
  -----------------------------------------------------------------------

**Method**

get_auth_server_by_id

**Endpoint**

**GET** /api/2/api_authorizations/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server to locate.     |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

create_auth_server

**Endpoint**

**POST** /api/2/api_authorizations

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

update_auth_server

**Endpoint**

**PUT** /api/2/api_authorizations/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the authorization server that    |
|                  | you want to update.                               |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  400         Bad Request               **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_auth_server

**Endpoint**

**DEL** /api/2/api_authorizations/:id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | Set to the id of the auth server you want to      |
|                  | delete.                                           |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_auth_claims

**Endpoint**

**GET** /api/2/api_authorizations/:id/claims

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

create_auth_server_claim

**Endpoint**

**POST** /api/2/api_authorizations/:id/claims

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+-------------------+--------------------------------------------------+
| Name/Type         | Description                                      |
+===================+==================================================+
| **name**          | The attribute name for the claim when its        |
|                   | returned in an Access Token                      |
| required          |                                                  |
|                   |                                                  |
| string            |                                                  |
+-------------------+--------------------------------------------------+
| **user_att        | A user attribute to map values from              |
| ribute_mappings** |                                                  |
|                   | For custom attributes prefix the name of the     |
| string            | attribute with \`custom_attribute\_\`.           |
|                   |                                                  |
|                   | e.g. To get the value for custom attribute       |
|                   | \`employee_id\`                                  |
|                   | use \`custom_attribute_employee_id\`.            |
+-------------------+--------------------------------------------------+
| **user_a          | When \`user_attribute_mappings\` is set to       |
| ttribute_macros** | \`\_macro\_\` this macro will be used to assign  |
|                   | the parameter value.                             |
| string            |                                                  |
+-------------------+--------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

update_claim

**Endpoint**

**PUT** /api/2/api_authorizations/:id/claims/:claim_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **claim_id**     | The id of the claim.                              |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+-------------------+--------------------------------------------------+
| Name/Type         | Description                                      |
+===================+==================================================+
| **name**          | The attribute name for the claim when its        |
|                   | returned in an Access Token                      |
| required          |                                                  |
|                   |                                                  |
| string            |                                                  |
+-------------------+--------------------------------------------------+
| **user_att        | A user attribute to map values from              |
| ribute_mappings** |                                                  |
|                   |                                                  |
| string            |                                                  |
+-------------------+--------------------------------------------------+
| **user_a          | When \`user_attribute_mappings\` is set to       |
| ttribute_macros** | \`\_macro\_\` this macro will be used to assign  |
|                   | the parameter value.                             |
| string            |                                                  |
+-------------------+--------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_auth_claim

**Endpoint**

**DEL** /api/2/api_authorizations/:id/claims/:claim_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **claim_id**     | The id of the claim.                              |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_auth_server_scopes

**Endpoint**

**GET** /api/2/api_authorizations/:id/scopes

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

create_auth_server_scope

**Endpoint**

**POST** /api/2/api_authorizations/:id/scopes

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+-------------------+--------------------------------------------------+
| Name/Type         | Description                                      |
+===================+==================================================+
| **value**         | A value representing the api scope that with be  |
|                   | authorized                                       |
| required          |                                                  |
|                   |                                                  |
| string            |                                                  |
+-------------------+--------------------------------------------------+
| **description**   | A description of what access the scope enables   |
|                   |                                                  |
| required          |                                                  |
|                   |                                                  |
| string            |                                                  |
+-------------------+--------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Ok                        **-**

  401         Unauthorized              **-**

  404         Not Found                 

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

update_auth_server_scope

**Endpoint**

**PUT** /api/2/api_authorizations/:id/scopes/:scope_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **scope_id**     | The id of the scope.                              |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+-------------------+--------------------------------------------------+
| Name/Type         | Description                                      |
+===================+==================================================+
| **value**         | A value representing the api scope that with be  |
|                   | authorized                                       |
| required          |                                                  |
|                   |                                                  |
| string            |                                                  |
+-------------------+--------------------------------------------------+
| **description**   | A description of what access the scope enables   |
|                   |                                                  |
| required          |                                                  |
|                   |                                                  |
| string            |                                                  |
+-------------------+--------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_auth_server_scope

**Endpoint**

**DEL** /api/2/api_authorizations/:id/scopes/:scope_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **scope_id**     | The id of the scope.                              |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

Get_client_apps

**Endpoint**

**GET** /api/2/api_authorizations/:id/clients

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

create_client_app

**Endpoint**

**POST** /api/2/api_authorizations/:id/clients

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+-------------------+--------------------------------------------------+
| Name/Type         | Description                                      |
+===================+==================================================+
| **app_id**        | The ID of the OpenId Connect app to allow        |
|                   | access through.                                  |
| required          |                                                  |
|                   |                                                  |
| integer           |                                                  |
+-------------------+--------------------------------------------------+
| **scopes**        | An array of Scope IDs that represent the scopes  |
|                   | the app can request                              |
| required          |                                                  |
|                   |                                                  |
| array             |                                                  |
+-------------------+--------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**

  404         Not Found                 

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

update_a_client_app

**Endpoint**

**PUT** /api/2/api_authorizations/:id/clients/:client_app_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| *                | The id of the client app.                         |
| *client_app_id** |                                                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+-------------------+--------------------------------------------------+
| Name/Type         | Description                                      |
+===================+==================================================+
| **scopes**        | An array of Scope IDs that represent the scopes  |
|                   | the app can request                              |
| required          |                                                  |
|                   |                                                  |
| array             |                                                  |
+-------------------+--------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_client_app

**Endpoint**

**DEL** /api/2/api_authorizations/:id/clients/:client_app_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **id**           | The id of the authorization server.               |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| *                | The id of the client app.                         |
| *client_app_id** |                                                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

17. **[Branding]{.underline}**

**Method**

list_account_brands

**Endpoint**

**GET** /api/2/branding/brands

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

create_brand

**Endpoint**

**POST** /api/2/branding/brands

**Request Parameters**

+-----------------------------+----------------------------------------+
| Name/Type                   | Description                            |
+=============================+========================================+
| **name**                    | Brand's name for humans. **This isn't  |
|                             | related to subdomains**.               |
| required                    |                                        |
|                             |                                        |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **master**                  | Establishes a primary master brand.    |
|                             |                                        |
| boolean                     |                                        |
+-----------------------------+----------------------------------------+
| **enabled**                 | Indicates if the brand is enabled      |
|                             | or not.(default=false)                 |
| boolean                     |                                        |
+-----------------------------+----------------------------------------+
| **custom_support_enabled**  | Indicates if the custom support is     |
|                             | enabled. If enabled, the login page    |
| boolean                     | includes the ability to submit a       |
|                             | support request.                       |
+-----------------------------+----------------------------------------+
| **custom_color**            | Primary brand color                    |
|                             |                                        |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **custom_accent_color**     | Secondary brand color                  |
|                             |                                        |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **custom_masking_color**    | Color for the masking layer above the  |
|                             | background image of the branded        |
| string                      | login screen.                          |
+-----------------------------+----------------------------------------+
| **custom_masking_opacity**  | Opacity for the custom_masking_color.  |
|                             |                                        |
| number                      |                                        |
+-----------------------------+----------------------------------------+
| **enable_cust               | Indicates if the custom Username/Email |
| om_label_for_login_screen** | field label is enabled or not.         |
|                             |                                        |
| boolean                     |                                        |
+-----------------------------+----------------------------------------+
| **custom_la                 | Custom label for the Username/Email    |
| bel_text_for_login_screen** | field on the login screen. See         |
|                             | example here.                          |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **login_instruction_title** | Link text to show login                |
|                             | instruction screen.                    |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **login_instruction**       | Text for the login instruction screen, |
|                             | styled in Markdown.                    |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **hide_onelogin_footer**    | Indicates if the OneLogin footer will  |
|                             | appear at the bottom of the            |
| boolean                     | login page.                            |
+-----------------------------+----------------------------------------+
| **mfa_enrollment_message**  | Text that replaces the default text    |
|                             | displayed on the initial screen of the |
| string                      | MFA Registration.                      |
+-----------------------------+----------------------------------------+
| **background**              | Base64 encoded image data              |
|                             | (JPG/PNG, \<5MB)                       |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **logo**                    | Base64 encoded image data (PNG, \<1MB) |
|                             |                                        |
| string                      |                                        |
+-----------------------------+----------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

get_brand_by_id

**Endpoint**

**GET** /api/2/branding/brands/:brand_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **brand_id**     | Unique identifier for the branding object.        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

update_brand

**Endpoint**

**PUT** /api/2/branding/brands/:brand_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **brand_id**     | Unique identifier for the branding object.        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+-----------------------------+----------------------------------------+
| Name/Type                   | Description                            |
+=============================+========================================+
| **name**                    | Brand's name for humans. **This isn't  |
|                             | related to subdomains**.               |
| required                    |                                        |
|                             |                                        |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **enabled**                 | Indicates if the brand is enabled      |
|                             | or not.(default=false)                 |
| boolean                     |                                        |
+-----------------------------+----------------------------------------+
| **custom_support_enabled**  | Indicates if the custom support is     |
|                             | enabled. If enabled, the login page    |
| boolean                     | includes the ability to submit a       |
|                             | support request.                       |
+-----------------------------+----------------------------------------+
| **custom_color**            | Primary brand color                    |
|                             |                                        |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **custom_accent_color**     | Secondary brand color                  |
|                             |                                        |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **custom_masking_color**    | Color for the masking layer above the  |
|                             | background image of the branded        |
| string                      | login screen.                          |
+-----------------------------+----------------------------------------+
| **custom_masking_opacity**  | Opacity for the custom_masking_color.  |
|                             |                                        |
| number                      |                                        |
+-----------------------------+----------------------------------------+
| **enable_cust               | Indicates if the custom Username/Email |
| om_label_for_login_screen** | field label is enabled or not.         |
|                             |                                        |
| boolean                     |                                        |
+-----------------------------+----------------------------------------+
| **custom_la                 | Custom label for the Username/Email    |
| bel_text_for_login_screen** | field on the login screen. See         |
|                             | example here.                          |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **login_instruction_title** | Link text to show login                |
|                             | instruction screen.                    |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **login_instruction**       | Text for the login instruction screen, |
|                             | styled in Markdown.                    |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **hide_onelogin_footer**    | Indicates if the OneLogin footer will  |
|                             | appear at the bottom of the            |
| boolean                     | login page.                            |
+-----------------------------+----------------------------------------+
| **mfa_enrollment_message**  | Text that replaces the default text    |
|                             | displayed on the initial screen of the |
| string                      | MFA Registration.                      |
+-----------------------------+----------------------------------------+
| **background**              | Base64 encoded image data              |
|                             | (JPG/PNG, \<5MB)                       |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **logo**                    | Base64 encoded image data (PNG, \<1MB) |
|                             |                                        |
| string                      |                                        |
+-----------------------------+----------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_brand

**Endpoint**

**DEL** /api/2/branding/brands/:brand_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **brand_id**     | Unique identifier for the branding object.        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| number           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  401         Bad Request               **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_brand_apps

**Endpoint**

**GET** /api/2/branding/:brand_id/apps

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **brand_id**     | Unique identifier for the branding object.        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| number           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

list_brand_templates

**Endpoint**

**GET** /api/2/branding/brands/:brand_id/templates

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **brand_id**     | Unique identifier for the branding object.        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Query Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **locale**       | The message locale to return.                     |
|                  |                                                   |
| optional         | Example:                                          |
|                  | https:///api/2/br                                 |
| integer          | anding/brands/:brand_id/templates?locale={locale} |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

create_brand_template

**Endpoint**

**POST** /api/2/branding/brands/:brand_id/templates

**Request Parameters**

+-----------------------------+----------------------------------------+
| Name/Type                   | Description                            |
+=============================+========================================+
| **type**                    | Template type that describes the       |
|                             | source (sms, voice, email) and purpose |
| required                    | (registration, invite, etc             |
|                             |                                        |
| string                      | Must be one of:                        |
|                             |                                        |
|                             | - email_forgot_password                |
|                             |                                        |
|                             | - email_code_registration              |
|                             |                                        |
|                             | - email_code_login_verification        |
|                             |                                        |
|                             | - email_code_app_verification          |
|                             |                                        |
|                             | - email_code_pw_reset_verification     |
|                             |                                        |
|                             | - email_magiclink_registration         |
|                             |                                        |
|                             | - email_magiclink_login_verification   |
|                             |                                        |
|                             | - email_magiclink_app_verification     |
|                             |                                        |
|                             | -                                      |
|                             |  email_magiclink_pw_reset_verification |
|                             |                                        |
|                             | - sms_registration                     |
|                             |                                        |
|                             | - sms_login_verification               |
|                             |                                        |
|                             | - sms_app_verification                 |
|                             |                                        |
|                             | sms_pw_reset_verification              |
+-----------------------------+----------------------------------------+
| **locale**                  | The 2 character language locale for    |
|                             | the template.                          |
| required                    |                                        |
|                             | e.g. en = English, es = Spanish        |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **template**                | The content body for the message. The  |
|                             | template parameters vary based on the  |
| required                    | type of template. Templates can also   |
|                             | contain placeholder values that will   |
| string                      | be substituted at the time of sending  |
|                             | a message.                             |
|                             |                                        |
|                             | **Email**                              |
|                             |                                        |
|                             | - subject - required - The             |
|                             |   email subject                        |
|                             |                                        |
|                             | - html - required - The HTML body of   |
|                             |   the email                            |
|                             |                                        |
|                             | - plain - required - The plain text    |
|                             |   body of the email                    |
|                             |                                        |
|                             | Placeholder values include url for     |
|                             | magiclink or forgot password types,    |
|                             | and otp_code for templates that have   |
|                             | the code type.                         |
|                             |                                        |
|                             | **SMS**                                |
|                             |                                        |
|                             | - message - required - The body of the |
|                             |   SMS message. Max length              |
|                             |   160 characters.                      |
|                             |                                        |
|                             | Placeholder values                     |
|                             | include otp_code for the OTP code      |
|                             | and otp_expiry for the time until the  |
|                             | code expires.                          |
+-----------------------------+----------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  201         Created                   **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

get_brand_templates

**Endpoint**

**GET** /api/2/branding/brands/:brand_id/templates/:template_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **brand_id**     | Unique identifier for the branding object.        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **template_id**  | Unique identifier for the template to return.     |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_brand_template_by_type

**Endpoint**

**GET** /api/2/branding/brands/:brand_id/templates/:template_type

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **brand_id**     | Unique identifier for the branding object.        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| *                | The message template type to return.              |
| *template_type** |                                                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_brand_template_by_type_and_locale

**Endpoint**

**GET** /api/2/branding/brands/:brand_id/templates/:template_type/:locale

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **brand_id**     | Unique identifier for the branding object.        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| *                | The message template type to return.              |
| *template_type** |                                                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **locale**       | The 2 character language locale for the template. |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_master_brand_template_by_type

**Endpoint**

**GET** /api/2/branding/brands/master/templates/:template_type

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| *                | The message template type to return.              |
| *template_type** |                                                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

get_master_brand_template_by_type_and_locale

**Endpoint**

**GET** /api/2/branding/brands/master/templates/:template_type/:locale

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| *                | The message template type to return.              |
| *template_type** |                                                   |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+
| **locale**       | The 2 character language locale for the template. |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| string           |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful Response       **-**

  401         Unauthorized              **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------

**Method**

update_brand_template

**Endpoint**

**PUT** /api/2/branding/brands/:brand_id/templates/:template_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **brand_id**     | Unique identifier for the branding object.        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **template_id**  | Unique identifier for the template to return.     |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**Request Parameters**

+-----------------------------+----------------------------------------+
| Name/Type                   | Description                            |
+=============================+========================================+
| **locale**                  | The 2 character language locale for    |
|                             | the template.                          |
| required                    |                                        |
|                             | e.g. en = English, es = Spanish        |
| string                      |                                        |
+-----------------------------+----------------------------------------+
| **template**                | The content body for the message. The  |
|                             | template parameters vary based on the  |
| required                    | type of template. Templates can also   |
|                             | contain placeholder values that will   |
| string                      | be substituted at the time of sending  |
|                             | a message.                             |
|                             |                                        |
|                             | **Email**                              |
|                             |                                        |
|                             | - subject - required - The             |
|                             |   email subject                        |
|                             |                                        |
|                             | - html - required - The HTML body of   |
|                             |   the email                            |
|                             |                                        |
|                             | - plain - required - The plain text    |
|                             |   body of the email                    |
|                             |                                        |
|                             | Placeholder values include url for     |
|                             | magiclink or forgot password types,    |
|                             | and code for templates that have the   |
|                             | code type.                             |
|                             |                                        |
|                             | **SMS**                                |
|                             |                                        |
|                             | - message - required - The body of the |
|                             |   SMS message. Max length              |
|                             |   160 characters.                      |
|                             |                                        |
|                             | Placeholder values                     |
|                             | include otp_code for the OTP code      |
|                             | and otp_expiry for the time until the  |
|                             | code expires.                          |
+-----------------------------+----------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  200         Successful response       **-**

  401         Unauthorized              **-**

  422         Unprocessable Entity      **-**
  -----------------------------------------------------------------------

**Method**

delete_brand_template

**Endpoint**

**DEL** /api/2/branding/brands/:brand_id/templates/:template_id

**Resource Parameters**

+------------------+---------------------------------------------------+
| Name/Type        | Description                                       |
+==================+===================================================+
| **brand_id**     | Unique identifier for the branding object.        |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+
| **template_id**  | Unique identifier for the template to return.     |
|                  |                                                   |
| required         |                                                   |
|                  |                                                   |
| integer          |                                                   |
+------------------+---------------------------------------------------+

**HTTP request headers**

**Content-Type:** not defined (recommended: application/json;
charset=utf-8)

**Accept:** application/json

**HTTP response details**

  -----------------------------------------------------------------------
  **Status    **Description**           **Response Headers**
  Code**                                
  ----------- ------------------------- ---------------------------------
  204         No Content                **-**

  404         Not Found                 **-**
  -----------------------------------------------------------------------
