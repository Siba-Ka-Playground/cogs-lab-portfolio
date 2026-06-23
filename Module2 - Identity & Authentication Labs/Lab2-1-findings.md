# Lab 2.1 Findings: LDAP & InstaSafe AD Sync

## 1. Mapping LDAP Concepts to InstaSafe AD Sync

Understanding how standard LDAP maps to InstaSafe's Active Directory synchronization is critical for configuring identity management correctly.

* **Base DN (Distinguished Name)**
    * **Lab Example:** `dc=lab,dc=instasafe,dc=local`
    * **InstaSafe Context:** The Base DN defines the starting point for InstaSafe's search within the Active Directory tree. If a company has a massive AD structure but only wants to sync users from a specific department, the Base DN would be narrowed down to that specific Organizational Unit (OU), e.g., `ou=Engineering,dc=company,dc=com`. 
* **Bind DN (Service Account)**
    * **Lab Example:** `cn=admin,dc=lab,dc=instasafe,dc=local`
    * **InstaSafe Context:** The Bind DN is the identity (usually a dedicated service account) that InstaSafe uses to log into the customer's Active Directory to read data. It requires sufficient read permissions over the Base DN to pull user and group records during the sync process.
* **Attributes**
    * **Lab Example:** `mail`, `uid`, `cn`, `sn`
    * **InstaSafe Context:** Attributes represent the specific data fields attached to a user object. During sync, InstaSafe must map AD attributes to its own internal database fields. For example, AD's `mail` attribute is mapped to the InstaSafe user's email address, and `sAMAccountName` or `uid` is mapped to the username used for VPN/ZTNA login.

## 2. Troubleshooting LDAP Bind Error 49

**Scenario:** An InstaSafe AD sync fails, and the logs display `ldap_bind: Invalid credentials (49)`.

**Support Engineer Action Plan:**
Error 49 strictly indicates an authentication failure during the Bind process. When a support engineer sees this in the AD sync logs, they know the connection to the server was successful, but the credentials were rejected. The engineer should immediately investigate the following:

1.  **Service Account Password:** The most common cause. The password for the Bind DN account may have expired, or an AD administrator may have changed it without updating the InstaSafe portal. The engineer will ask the client to re-enter the Bind password in the InstaSafe configuration.
2.  **Account Lockout:** The Bind DN service account may be temporarily locked out in Active Directory due to too many failed password attempts. The engineer will ask the AD admin to check the account status.
3.  **Invalid Bind DN Format:** Ensure the Bind DN is formatted exactly as AD expects. While standard LDAP uses full DNs (e.g., `cn=syncuser,ou=services,dc=domain,dc=com`), sometimes configurations expect a User Principal Name (UPN) like `syncuser@domain.com` or down-level logon name `DOMAIN\syncuser`.
4.  **Disabled Account:** The service account used for the Bind DN may have been disabled by the client's IT team during a security audit.
lab2-1-findings.md
Displaying lab2-1-findings.md.
