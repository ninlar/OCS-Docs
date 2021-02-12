---
uid: communityroles
---

# Community roles

When using Communities, you must [assign users to roles](xref:managecommunityusers#assign-user-roles) that grant permissions for certain types of actions.

In Communities, the following roles are possible, ordered from least privileged to most privileged:

## Community Member

The role representing a user in the community authorized to read any data shared in the community.

## Data Steward

A role granted the privilege to share a tenant’s data with any community to which the data steward belongs. A data steward can grant this sharing privilege (that is, the data steward role) to other users in their own tenant.

## Community Moderator

A role assigned to a user within a community member tenant who manages the community for the tenant. Usually, a Community Moderator is the person who accepted the invitation on behalf of his or her tenant. This user must also be an Tenant Administrator within their own tenant.

The Community Moderator's primary purpose is to manage what users from their tenant have access to the community. Community Moderators have all the privileges of a Community Member, plus the ability to:  

- Pause access to shared data from their tenant.
- Add users from their tenant to the community or remove users from the community
- Remove their tenant from the community

A Community Moderator can assign the following roles to users in their own tenant:

- Community Member
- Community Moderator

## Community Administrator

A role assigned to a user granting full administrative rights. These rights include all the privileges granted to a Community Moderator plus the ability to:

- Delete the community
- Invite and remove tenants from the community

Community Administrators in the community owner's tenant can assign any role to any user in any tenant belonging to the community. A community owner is the tenant who created a community.

Community Administrators in tenants that *do not belong* to the community owner's tenant can assign any role to users in their own tenant.

## Related concepts

In addition to the roles you can assign in Communities, be aware of the following related concepts:

### Tenant Administrator

A tenant-level role that allows users to administer their own tenant. Tenant Administrators are roles that apply to all of OCS rather than just to Communities. In OCS, user roles are managed as part of the Security component.

### Community Owner

The tenant who created a community. A community owner is not a role and is not granted any privileges. By default Tenant Administrators are granted the privilege to create communities.