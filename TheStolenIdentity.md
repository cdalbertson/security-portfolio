# Investigation of Access Breach & Reconstruction of Kill-Chain: Findings and Recommendations

## Scenario
This is an investigation into a security breach that was not detected by existing security policy or governance.  The client security team identified tampering with a legacy app and flagged new changes.  The incident team recorded the access method in metadata to assist analysis.  Requirements of the investigation are to conduct analysis of the client system and reconstruct the kill-chain used by the intruder.

## Environment
Live multi-user Azure training tenant, Reader access

## Investigation
1. Entry: I began my investigation by checking the information provided by the security and incident teams.  Navigating to the noted app registration and the metadata in the branding & properties tab, I found that the initial access was gained when an employee was victim of a phishing attack.  The phish directed the employee to a fake login where they entered their credentials and provided the MFA response when attempting to login to the false portal.  This phishing allowed the actor to avoid detection by using legitimate credentials and a valid session token.
 <img width="1567" height="632" alt="Image" src="https://github.com/user-attachments/assets/449d4c34-7c97-4200-ad2a-6b07e6186b16" />
 
2. Escalate: The compromised employee did not carry any privileged access entitlements across the tenant, but was the owner of the compromised app.  This app is a legacy app and the employee retained owner status had been overlooked by the client.  The actor identified this excessive entitlement and by checking the certificates and secrets tab they used the employee's owner status to issue new API credentials through the app.  This enabled the actor to bypass the access restrictions inherent to the employee as the new credentials elevated the entitlement level to that of service principal and also bypassing the need for further use of a phished session token.  The expiration of the issued credentials indicates an intended persistence method.
<img width="1578" height="637" alt="Image" src="https://github.com/user-attachments/assets/421df51a-c04a-42f7-93f4-cec004dd196d" />

3. Pivot: With the new entitlements inherited from owner status the actor shifted to embed themselves further into the tenant  The actor did this by creating a new app and assigned the service principle of the new app as an owner of the legacy app.  In API permissions we can see that the requirement for Admin consent is enabled, illustrating how the errant owner status was sufficient to create a cascading process of embedding elevated access was exploited by the actor.
<img width="1456" height="668" alt="Image" src="https://github.com/user-attachments/assets/f94b80dc-3a67-4242-bd10-e42b9d4d766d" />

4. Persistence: Within the created app in the expose an API tab we an see a new scope was created.  The scope designates the legacy app as a secure resource enabling the rogue app to request delegated access.  This is where a deeper persistence is enabled.  The actor created a scenario where even if the employee owner entitlement was cleaned up and the new CID with excessive expiration were removed/rotated, that the actor could simply provision new credentials as needed and reinsert themselves.
<img width="1456" height="663" alt="Image" src="https://github.com/user-attachments/assets/45f0093b-5e27-44a2-a330-2991e2c4f422" />

5. Objective: Checking the illegitimate app we can see where the actor's intentions pointed to.  Through chaining permissions, and the new over provisioned scope, and the disguised authentication pathing through rogue app's redirect URL allowing the legacy app will allow the actor to harvest session tokens.  This is the end of what my analysis found as the threat actor's objective so far as they were able to establish themselves before their presence was identified.
<img width="1456" height="607" alt="Image" src="https://github.com/user-attachments/assets/69b4a025-ef8e-4ba6-9e04-9606de69aee2" />

## What broke / what surprised me
1. I was surprised to learn about the carried level of entitlement through the steps of the kill chain and how that is exploitable through a basic understanding of how provisioning works within Azure.
2. I did not anticipate that as a default any user can register an app.  I would have thought for certain that inherent design would necessitate establishing the highest level access and entitlements and that subsequent access would default to zero access.
3. Being a new learner to cyber security it was interesting to see how redirect URI can be exploited from within a system like this and bypass the security policies on users that establish the perimeter, including MFA tokens.

## Findings and recommendations
Recommendations are as follows:
1. Conduct an audit of all registered app owners and remove unnecessary entitlements/access.
2. Remove/delete all found changes enacted by the threat actor including the following: remove any OAuth2PermissionGrant associated, remove the redirect URI, remove the custom scope, review and reduce capability for the Graph permissions, remove the new client secret, remove the rogue service principle owner from the legacy app
3. Force password update on the phishing victim
4. Enact alerting on new client secret provisioning, app registration, redirect configuration
5. Change default setting to disable app registration for all users.
6. Conduct employee training

## What I learned
1. That IAM within organizations needs scheduled reviews of established users and applications to clean up old and/or unnecessary access and entitlements
2. I learned how relaxed or rushed security standards on what appear to be small risks can actually carry significant impact radius by exploiting overlooked or unmanaged access.
3. The default setting for all users being able to register an app is something that should be on any cloud security team's checklist to create and enforce policy which requires inspection and approval.
4. Additional navigation of the Azure system
