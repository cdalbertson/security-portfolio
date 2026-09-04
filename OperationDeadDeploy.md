# Investigating an Unauthorized Resource Provision: Findings and Recommendations

## Scenario
This is an investigation into an unauthorized resource deployment was detected which should have been prevented by policy.  Requirements are to to find which user executed the resource deployment and identify any governance failures which permitted the deploy.  All findings are to be documented with evidence and accompanied with any recommended actions based on those findings.



## Environment
Live multi-user Azure training tenant, Reader access

## Investigation
1. I began by investigating the tenant resource groups to identify any which may not conform to policy naming standards or had other immediate abnormalities.  There was a resource group found on page 2 which didn't match the naming convention used on the other resource groups, which prompted scrutiny.




2. Inspection of that resource group showed that there was a resource within the resource group.  Inspecting the group for tags, there was a tag associated which did give information for the resource owner.  With this there was now evidence of the likely resource group, resource, and owner being identified.





3. Investigation of the deployment was the next area to inspect for evidence regarding the unauthorized provisioning to confirm prior findings as well as begin looking for the governance failures which allowed it to happen.  I was able to confirm the deployment and associated details and proceeded to investigate the possible causes which allowed the unauthorized provisioning starting with inspection of enacted policy.




4. Inspection of the non-compliant value confirmed the deploy suspected was not in line with policy, but was not stopped which prompted inspection of the policy itself.



5. Inspection of the JSON for the policy configuration revealed that this policy has 3 values allowed and a default value of Audit.  Which confirms that this is the policy configuration value which allowed the governance failure that permitted the deployment of a non-conforming deployment.


## What broke / what surprised me
Inspecting the JSON to find that it had multiple allowed values and a default that was the least restrictive was a bit surprising.  I wanted to be certain that the chain of events could be substantiated on the evidence gathered and that the details which turned my suspicions into valid affirmed my findings through each step, this took longer than I anticipated.

## Findings and recommendations
Recommendations are as follows:Configure the failed policy to the most restrictive setting.  If this is not acceptable, then the default value should be configured to the most restrictive setting to act as another level of action required.  Enact controls and policy on users to restrict on basis of time/day to prevent issues from incurring costs over weekends.  Supplemental training of juniors and interns could act as another measure to prevent similar issues in the future.

## What I learned
1. I learned that doing my attempt at as close to professional an investigation with matching soundness of reasoning and evidence is something that does force writing in a different tone and logical structure which was almost more difficult than the investigation itself  Especially when keeping in mind that reports are written for the reader, so keeping a level of ease in following should be important.
2. I had my first exposure to Microsoft Azure which was unexpectedly intuitive for navigating and I oddly kind of liked it although I know the tool has massive complexity.
3. I also had my first experience inspecting JSON for my own purposes, despite not yet having actually written anything with JSON myself.  I understand that might be strange to read, but I enrolled in this to challenge myself to move forward aggressively while I pursue basic certifications with the hope I'll come out with more functional knowledge and documented account of my willingness to be challenged in a very real way and seeing how I meet those difficulties.
4. Something I would do differently is I would have started with Policy next time to see what if any violations there were rather than, depending on how much information is/isn't provided, looking through a potentially very long list of resource groups.
5. <img width="1327" height="581" alt="1 azure_main" src="https://github.com/user-attachments/assets/00b54689-45cc-483a-8091-256efc66fcb9" />
I am surprised to find that this was kind of fun for me and I'm not quite sure what that could indicate for me as I look for clues as to what I should pursue as an initial specialization or focus.
