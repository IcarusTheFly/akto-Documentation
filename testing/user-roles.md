# Test roles

If you have role-based access control in your APIs and application, you would want to test for privilege escalation vulnerabilities. This typically involves checking access of all the higher-privilege APIs by a lower-privilege user. If any such API exists, then it's privilege should be fixed by the development team.

Akto enables you to do automate privilege escalation security tests. You can also use the information while creating Test YAML templates if you want to develop custom tests.

## Testing for Privilege Escalation using Akto

1. Set up all your application roles in Akto along with the auth token for each role (hardcoded or automated). Explained [here](user-roles.md#configure-application-roles)
2. For each role, find all the APIs that are accessible by that role. Explained [here](user-roles.md#create-access-matrix)
3. Setup automated test to check if any API exclusive to a higher-privilege role can be accessed by a lower-privilege role. Explained [here](user-roles.md#create-test-template)

The instructions here cover the above 3 steps in detail.

## Configure application roles

You should create as many roles as are in your application. For example, if you are a B2B Saas app, you might have ADMIN, BILLING, MEMBER, GUEST etc. roles. Role creation is a one-time configuration.

### Create role

1.  In the left navigation menu, go to `Testing` and then click on `Roles` tab.&#x20;

    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/5ff6cae1-f4a2-4179-843c-c77cc11da9c8" alt=""><figcaption></figcaption></figure>
2.  Click on the plus-icon to add a new role.&#x20;

    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/6f8baaa1-f9fc-44b7-b303-021da6e66e6e" alt=""><figcaption></figcaption></figure>
3.  Give a suitable name to your role eg. ADMIN. Select the APIs that belong to the application. If you want to select from an existing API Collection, use `belongs to` operator.&#x20;

    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/7dcf85e2-760a-437a-a642-7283662710de" alt=""><figcaption></figcaption></figure>
4.  Select API Collection from the dropdown menu.



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/3feceb3d-f4c4-42db-83f3-f7177b45aec7" alt=""><figcaption></figcaption></figure>
5.  Select endpoints you'd want to evaluate. You can simply select all. And click on `Save` button.



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/e16c7d60-55de-404e-8592-c47f0e326e7b" alt=""><figcaption></figcaption></figure>

### Add hard-coded auth token for the role

During API testing, Akto will add these headers to the sample API calls to check if the role has access to the API. You can provide a hardcoded auth header or you can also automate fetching auth token automatically (next section) for the role.

1.  Click on the `Add auth` button to add auth-token for your role. Then click on the `plus` icon to add auth headers for the role



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/48c733d1-94a7-4f76-97d6-79fb724724e7" alt=""><figcaption></figcaption></figure>
2.  Add your auth header key and its value as shown.&#x20;

    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/ebe48802-dd9e-477d-a4ed-d09146c680f5" alt=""><figcaption></figcaption></figure>
3.  You can add multiple headers too. Click on the `plus` icon to add more headers for this role. Click on `Save` when you are done.



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/b175c895-f03d-4081-9cb6-89e8423ae025" alt=""><figcaption></figcaption></figure>

### Automate auth token for the role

If you configure automated auth token generation, Akto will run the Login-request sequence before the test starts and fetch the auth token. You should know which API (or sequence of APIs) can give you the auth token in the response. For this example, we will configure a Login-request sequence of 2 APIs. The response of the 2nd API contains the auth token.&#x20;

<figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/ecf4167a-8f3c-4622-ab33-756e4cca5b74" alt=""><figcaption></figcaption></figure>

1.  If you want to automate auth token generation you can select `Login-request` radio button. And click on `Create` button.



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/0a9d781d-eb84-4ccf-a310-cd1d7e7befa1" alt=""><figcaption></figcaption></figure>
2.  You will see a dialog box where you can configure the sequence to fetch auth token automatically.



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/78750231-5da8-41df-b3af-1851d8067aca" alt=""><figcaption></figcaption></figure>
3.  You can set URL, Query params, Method, Headers and Body for your request. Click on the `edit` button on each field to set it. Once you enter the required value, click on the `check` icon at the end of the line to save the field.



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/3b72eacb-4415-451a-bb72-c096cc14c833" alt=""><figcaption></figcaption></figure>
4.  Here, we have configured the first API call to create a user with a given email and password. You can choose to skip this api call in your case if you already have a user created with this role.



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/a868ff02-3427-4917-9072-d049587379eb" alt=""><figcaption></figcaption></figure>
5.  Click on the `Test` button to see the result of this API call.&#x20;

    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/2916ba03-5989-4249-90f4-bfd4c07543a8" alt=""><figcaption></figcaption></figure>
6. If you have configured the API call correctly, you should see the desired response. In this case, we see the API responds with success.
7.  To add another API call to the login sequence, click on the `Add step` button. You can multiple steps for the login sequence. If you want to remove a step, click on `Remove step` button on the top-right.



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/24ec4c80-6741-4894-9bc0-73b5b7b2a36f" alt=""><figcaption></figcaption></figure>
8.  Let's configure the 2nd API call in the login sequence. Note that the `email` and `password` field should be the same as previous API call. In the sequence, you can substitute values from requests & responses of previous API calls. You can refer to the first API call as `x1`. You can access `request` or `response`. In `request`, you can access `url`, `method`, `body`, `headers` fields. For `response`, you can access `body` and `headers` field. Remember to use `${}` while referring to any of API variables. For example, to get email from the request body of the first API call, we can say `${x1.request.body.email}`. Similarly we can access password field using `${x1.request.body.password}`



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/357925f6-b8c2-4969-9b13-3940edb4af0f" alt=""><figcaption></figcaption></figure>
9. Click on the `Test` button to check for the response. Here, we successfully get the auth token in the response body. Click on `Extract` button to start extracting token from this API response.

<figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/44115c4f-2015-49cb-acad-1fee1dc71916" alt=""><figcaption></figcaption></figure>

10. Under "Header value" mention how to extract the auth token from the API response. In this example we can say `${x2.response.body.authentication.token}`. Mention the "Header key" under which Auth token will be attached as part of the API request while testing. eg. `Authorization`. Click on `Done` button.



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/1b3d890c-0615-426a-813b-da1a7fc546df" alt=""><figcaption></figcaption></figure>
11. If the auth token is supposed to be attached in the request body, you can select `Body` in the step above.
12. Don't forget to `Save` the automated login sequence in the role.



    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/a4ee8861-68fc-4c92-ad83-e61621856cb9" alt=""><figcaption></figcaption></figure>

### If your sample requests come from diff accounts

Akto will replay the API requests which it recorded in your application server traffic. In your setup, it might be possible that there are multiple accounts being used on your staging app, hence Akto will record API calls from multiple accounts. In these cases, just 1 auth token per role might not suffice. You might want to configure 1 auth token for each account. When Akto tries to replay the recorded sample API call, it can pick up the auth token meant for the same account as the API request to check for role-access.

1.  If account id comes as a simple header field, you can click on the `Analyze` tab to find out possible values of any header that Akto has captured.&#x20;

    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/ee147220-e4cb-40a1-9bac-ac14efed8ef5" alt=""><figcaption></figcaption></figure>
2.  For example, we can find out how many diff values has Akto recorded with `CompanyId` header. We see only 3 "CompanyId" values. Note that the analysis comes from saved requests only. Akto doesn't save all API traffic it processes. It saves only a few requests.&#x20;

    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/f734536d-2d71-4a1c-abe1-c971fcaaba6f" alt=""><figcaption></figcaption></figure>
3.  We should configure an "auth" token for each such company. We can use `Api Header conditions` for this. Mention "CompanyId" as header key and "1234567" as header value. Akto will use this token while testing for sample API requests whose CompanyId in the headers equals "1234567". We should configure 2 more auths for remaining 2 companies for complete coverage.&#x20;

    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/0a8fcc6e-7dc4-42f1-a617-e9e473aaadd4" alt=""><figcaption></figcaption></figure>

## Create access matrix

Once tokens are set up, the next step is to hit all the recorded APIs with tokens from each role. This way, Akto maintains a table for each role for each API - if the role can access that API. We call this `Access matrix`.

1.  Click on the `Access` tab&#x20;

    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/1813f7dc-c5d2-45ca-acac-8ff12c1bd2c3" alt=""><figcaption></figcaption></figure>
2. Click on `Create access matrix` button
3.  Currently, access-matrix job runs once per hour. You might have to wait for an hour to see the results. You don't have to click on "Create access matrix" button for other roles one-by-one. Akto will create access matrix for all roles together.  In this example of Juiceshop, ADMIN can access 52 APIs, but MEMBER can access 43. There are 9 APIs exclusive to Admin.

    <figure><img src="https://github.com/akto-api-security/Documentation/assets/91221068/9798a99a-cff6-47aa-a95c-f53524c2ea1f" alt=""><figcaption></figcaption></figure>
4. Akto will run this task daily.

You should periodically review the access matrix. Specially when any associations change.

## Create test template

You can use this information in test YAML templates too. You can filter APIs _exclusive_ to the ADMIN role (9 APIs in this example). You can also get the auth token for any role in the template.

Say we want to create a custom YAML test that should run on the Admin-exclusive APIs using Member token. If any API returns 2XX response, we will send an alert.

### Filter Admin-exclusive APIs

This is a combination of 2 conditions - The API should accessible by ADMIN, and, the API should NOT be accessible by MEMBER.

* `include_roles_access` - You can use this filter to include all the APIs accessible by a particular role. eg. here we can use it as -

```
    include_roles_access:
    param: ADMIN
```

* `exclude_roles_access` - You can use this filter to exclude all the APIs accessible by a particular role. eg. here we can use it as -

```
    exclude_roles_access:
    param: MEMBER
```

You `api_selection_filters` section can then look like the following -

```
api_selection_filters:
  response_code:
    gte: 200
    lt: 300
  include_roles_access:
    param: ADMIN
  exclude_roles_access:
    param: MEMBER
```

### Use auth token from the role

You can use the auth token from any role in your test template. In our example, we would want to get the token of the MEMBER role. We can use `modify_header` rule. We can pass `${roles_access_context.MEMBER}: 1` as its value which will obtain an auth token of MEMBER role and replace in the API call while testing.

Your execute section will then look like -

```
execute:
  type: single
  requests:
  - req:
    - modify_header:
        ${roles_access_context.MEMBER}: 1
```

You can save this test template with name & id as "PRIVILEGE\_ESCALATION\_TEST" and have it part of your CI/CD pipeline. Now no API goes without being tested for privilege escalation!
