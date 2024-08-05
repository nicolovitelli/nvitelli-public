---
title: Managing Users
---

## WITH GRANT OPTION vs. WITH ADMIN OPTION

**WITH GRANT OPTION**
*Only for OBJECT PRIVILEGES.*
The owner of an object can grant it to another user by specifying the WITH GRANT OPTION clause in the GRANT statement.\
The new grantee can then grant the same level of access to other users or roles.

**Rules**
- You cannot grant WITH GRANT OPTION to a role.
- If you revoke access to a user who had been granted access to an object WITH GRANT OPTION and that user had granted access to another user, both sets of grants will be revoked (cascade effect).

**WITH ADMIN OPTION**
*Only for SYSTEM PRIVILEGES.*
Specify WITH ADMIN OPTION to enable the grantee to:
- Grant the role to another user or role, unless the role is a GLOBAL role
- Revoke the role from another user or role
- Alter the role to change the authorization needed to access it
- Drop the role

To revoke the ADMIN OPTION on a Sytem Privilege or role from a user, you must revoke the privilege or role from the user altogether and then grant the privilege or role to the user without the ADMIN OPTION.