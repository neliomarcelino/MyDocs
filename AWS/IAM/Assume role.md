# Assume role

> A role specifies a set of permissions that you can use to access AWS resources. In that sense, it is similar to an IAM user. An application assumes a role to receive permissions to carry out required tasks and interact with AWS resources. The role can be in your own account or any other AWS account. For more information about roles, their benefits, and how to create and configure them, see IAM Roles, and Creating IAM Roles. To learn about the different methods that you can use to assume a role, see Using IAM Roles.

>[!Important]
>The permissions of your IAM user and any roles that you assume are not cumulative. Only one set of permissions is active at a time. When you assume a role, you temporarily give up your previous user or role permissions and work with the permissions assigned to the role. When you exit the role, your user permissions are automatically restored. 
>To assume a role, an application calls the AWS STS AssumeRole API operation and passes the ARN of the role to use. When you call AssumeRole, you can optionally pass a JSON policy. This allows you to restrict permissions for that for the role’s temporary credentials. This is useful when you need to give the temporary credentials to someone else. They can use the role’s temporary credentials in subsequent AWS API calls to access resources in the account that owns the role.
>You cannot use the passed policy to grant permissions that are in excess of those: allowed by the permissions policy of the role that is being assumed. To learn more: about how AWS determines the effective permissions of a role, see Policy Evaluation Logic.
>
>[More info](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-api.html)