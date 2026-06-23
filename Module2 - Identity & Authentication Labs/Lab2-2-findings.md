# Lab 2-2 Findings: Keycloak SAML Configuration

## Attribute Mapper Explanation
In this lab, we configured attribute mappers within Keycloak. The purpose of these mappers is to bridge the gap between Keycloak's internal user database and the SAML assertions expected by the Service Provider (SP). By mapping the internal User Property `email` to the SAML Attribute Name `email`, and doing the same for `groups`, we ensure that when Keycloak generates the SAML response upon a successful login, it explicitly includes the user's email address and group memberships. The Service Provider relies on this injected metadata to apply role-based access control (RBAC) and properly identify the user within its own system.

## SSO Troubleshooting Answer

**Scenario:** *If a customer reports SSO users get an 'Attribute Error', what in this Keycloak config would you check first?*

If a Service Provider throws an "Attribute Error" during an SSO login flow, it almost always means the Identity Provider (Keycloak) is failing to send a specific piece of user data (like an email address, role, or group) that the SP strictly requires to complete the login process, or it is sending it under an unexpected naming convention.

**What to check first:**
I would immediately check the **Client Scopes and Mappers** configuration to ensure the required attributes are being attached to the SAML assertion.

**Specific Menu Path to check in Keycloak:**
1. Log into the Keycloak Admin Console.
2. Ensure you are in the correct Realm (e.g., `instasafe-lab`).
3. Navigate to **Clients** in the left-hand menu.
4. Select the specific client experiencing the issue (e.g., `https://sp.instasafe.local/saml`).
5. Navigate to the **Client scopes** tab for that client.
6. Click on the dedicated scope for that client.
7. Click on the **Mappers** tab.

**Things to verify here:**
* Ensure that mappers exist for all attributes the SP expects (e.g., if the SP needs an email, an email mapper must exist).
* Verify that the **"SAML Attribute Name"** field exactly matches the string the Service Provider is looking for. SAML is case-sensitive and strict; if the SP expects `User.Email` but Keycloak is sending `email`, an attribute error will occur.
