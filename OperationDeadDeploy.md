# Investigating an Unauthorized Resource Provision: Findings and Recommendations

## Scenario
This is an investigation into an unauthorized resource deployment was detected which should have been prevented by policy.  Requirements are to to find which user executed the resource deployment and identify any governance failures which permitted the deploy.  All findings are to be documented with evidence and accompanied with any recommended actions based on those findings.



## Environment
Live multi-user Azure training tenant, Reader access

## Investigation
1. I began by investigating the tenant resource groups to identify any which may not conform to policy naming standards or had other immediate abnormalities.
<img width="1327" height="581" alt="Image" src="https://github.com/user-attachments/assets/8fba89d4-ee8e-435f-aaab-b5afef023464" />
 
2. There was a resource group found on page 2 which didn't match the naming convention used on the other resource groups, which prompted scrutiny.
<img width="1187" height="562" alt="Image" src="https://github.com/user-attachments/assets/b87fc2f7-89b8-4f63-96ea-9572ed9115bf" />

3. Inspection of that resource group showed that there was a resource within the resource group.
<img width="1169" height="550" alt="Image" src="https://github.com/user-attachments/assets/db59f651-f59c-49e2-91a2-c8100ffd560d" />

4. Inspecting the group for tags, there was a tag associated which did give information for the resource owner.  With this there was now evidence of the likely resource group, resource, and owner being identified.
<img width="1161" height="541" alt="Image" src="https://github.com/user-attachments/assets/f7b4abb2-c47e-4f8d-b6da-14f390f550d6" />

5. Investigation of the deployment was the next area to inspect for evidence regarding the unauthorized provisioning to confirm prior findings as well as begin looking for the governance failures which allowed it to happen.  I was able to confirm the deployment and associated details and proceeded to investigate the possible causes which allowed the unauthorized provisioning starting with inspection of enacted policy.
<img width="1176" height="393" alt="Image" src="https://github.com/user-attachments/assets/ba03e29e-d38b-400f-b647-5bc63d2ec2a2" />

6. Inspection of the non-compliant value confirmed the deploy suspected was not in line with policy, but was not stopped.
<img width="1146" height="487" alt="Image" src="https://github.com/user-attachments/assets/6c6b7e6d-5310-48ff-9578-6ed57ca16a84" />

7. This prompted my inspection of the policy itself.
<img width="1178" height="522" alt="Image" src="https://github.com/user-attachments/assets/cbe03563-a7c8-47c6-8a84-532647429013" />

5. Inspection of the JSON for the policy configuration revealed that this policy has 3 values allowed and a default value of Audit.  Which confirms that this is the policy configuration value which allowed the governance failure that permitted the deployment of a non-conforming deployment.
<img width="1186" height="535" alt="Image" src="https://github.com/user-attachments/assets/22fb665b-e633-4295-aa3f-6be397df2f77" />

## What broke / what surprised me
Inspecting the JSON to find that it had multiple allowed values and a default that was the least restrictive was a bit surprising.  I wanted to be certain that the chain of events could be substantiated on the evidence gathered and that the details which turned my suspicions into valid affirmed my findings through each step, this took longer than I anticipated.

## Findings and recommendations
Recommendations are as follows:Configure the failed policy to the most restrictive setting.  If this is not acceptable, then the default value should be configured to the most restrictive setting to act as another level of action required.  Enact controls and policy on users to restrict on basis of time/day to prevent issues from incurring costs over weekends.  Supplemental training of juniors and interns could act as another measure to prevent similar issues in the future.

## What I learned
1. I learned that doing my attempt at as close to professional an investigation with matching soundness of reasoning and evidence is something that does force writing in a different tone and logical structure which was almost more difficult than the investigation itself  Especially when keeping in mind that reports are written for the reader, so keeping a level of ease in following should be important.
2. I had my first exposure to Microsoft Azure which was unexpectedly intuitive for navigating and I oddly kind of liked it although I know the tool has massive complexity.
3. I also had my first experience inspecting JSON for my own purposes, despite not yet having actually written anything with JSON myself.  I understand that might be strange to read, but I enrolled in this to challenge myself to move forward aggressively while I pursue basic certifications with the hope I'll come out with more functional knowledge and documented account of my willingness to be challenged in a very real way and seeing how I meet those difficulties.
4. Something I would do differently is I would have started with Policy next time to see what if any violations there were rather than, depending on how much information is/isn't provided, looking through a potentially very long list of resource groups.
5. I am surprised to find that this was kind of fun for me and I'm not quite sure what that could indicate for me as I look for clues as to what I should pursue as an initial specialization or focus.
