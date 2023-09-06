# Workato Recipe

Auto add/update netsuite employees to expensify</br>
For security measures, I cannot share the .json for this recipe - let me know if you need guidance on this topic !

## Workato actions explained

1. Trigger if netsuite employee is added/created & "Expensify User" is True
2. If netsuite site data is missing OR netsuite user is inactive, update the netsuite field "Expensify User" to False, and STOP job
5. Get applicable expensify policy per netsuite employee's site (Python)
6. Determine employee's supervisor and store the supervisor netsuite internal id (Ruby)
7. create admin employees list (to prevent over-riding their permissions if they are going to be added/updated as users/admins)
8+9. Get Netsuite Supervisor using the supervisor netsuite internal id from step 6, and store his data in applicable variables (supervisor_internal_id, supervisor_email, supervisor_site)
10+11. If Supervisor is not eligible for expensify (Depending on site) then he should not be added in expensify as supervisor, replace him with primary expenisfy admin as the supervisor of the employee.
12+13. If the employee is not eligible for expensify, update the netsuite field "Expensify User" to False, and STOP job
15. Per the supervisor_site, get the supervisor local expensify policy id (python)
16. create variable requestJobDescription_get_policy_users - this will store the request body for the HTTP response action that will be used in step 17
16. create given_user - will store the user json data that should be posted to expensify (role, email, supervisor, forwardTo), will be used to check if the user data in the recipe matches the existing data in expensify policy, if not then the given_user will be added/updated to the policy
16. create given_employee_supervisor - will store the supervisor user data (role, email, supervisor, forwardTo), will be used to check if the supervisor user data in the recipe matches the existing data in expensify policy, if not then the given_employee_supervisor will be added/updated to the policy
17. Get all users in the given policy (HTTP request)
18. Convert json data from step 17 to list (python)
19-26. Compare supervisor's local policy with the user's expensify policy, if not equal then add the supervisor to the user's policy with admin access if the supervisor exists in the admin employee list  (from step 7), else add supervisor with regular access. And finally update the NetSuite "Expensify User" checkbox to True.
29. check if given_user exists in the list created from step 18, if exists (match) then Stop Job, else add/update given_user in the expensify policy with admin access if the user exists in the admin employee list (from step 7), else add user with regular access.

## NetSuite ERP

For better automation, this recipe works simultaneously with netsuite workflows that automatically set "Expensify User" as True once the employee is hired. 

## Author

Jalil

