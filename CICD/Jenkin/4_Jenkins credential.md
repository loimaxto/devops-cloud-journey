### Jenkins Credential Scopes

- Jenkins uses scopes to limit access to sensitive data following the "Principle of Least Privilege."
- Prefer authenticate using file

| Scope      | Accessibility                                                                          | Best Use Case                                                      |
| :--------- | :------------------------------------------------------------------------------------- | :----------------------------------------------------------------- |
| **Global** | Available to all Jobs and Pipelines across the entire server.                          | Git tokens, Docker credentials, and general CI/CD tasks.           |
| **System** | Only available in Jenkins server (Not Jenkins job)                                     | SSH keys for agents, Cloud provider plugins (AWS/Azure), and LDAP. |
| **Folder** | Accessible only by Jobs residing within that specific folder. (Job --> Job credential) | Isolating team secrets in shared environments (Multi-tenancy).     |

**Key Takeaway:** Most developers should use **Global** for standard pipelines. Administrators should use **System** for infrastructure configurations.