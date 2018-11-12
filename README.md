# AdobeUM

Adopted script originally created by zincarla (https://github.com/zincarla/AdobeUMInterface) and converted it to a PowerShell Module for Azure Automation, providing cmdlets to communicate with the Adobe User Management API.

Available from PowerShell Gallery:</br>
https://www.powershellgallery.com/packages/AdobeUM

## Example Runbook </br>
See the following link for an example Runbook the Syncs an Azure AD Group with an Adobe Group:</br>
https://github.com/mrptsai/AdobeUMRunbook

## Functions

### Import-PFXCert
**Description**</br>
Import a certificate with a private key from file 

**Parameters**
- **CertPwd** - Password to open PFX Bitbucket UserID and Password
- **CertPath** - Path to PFX File (ClientID) and Secret

**Examples**
```PowerShell
    Import-PFXCert `
        -CertPwd "ASDF"     
        -CertPath "C:\Cert.pfx"
```

### ConvertTo-Base64URL
**Description**</br>
Converts a byte[], to a Base64URL encoded string

**Parameters**
- **Item** - A byte[]

**Examples**
```PowerShell
    ConvertTo-Base64URL `
        -Item "VGhpcyBpcyBhIHRlc3Q="
```
### ConvertFrom-Base64URL
**Description**</br>
Converts a Base64Url string, to a decoded ASCII string.               

**Parameters**
- **Item** - A base64url string Bitbucket Account

**Examples**
```PowerShell
    ConvertFrom-Base64URL `
        -Item "VGhpcyBpcyBhIHRlc3Q"
```
### ConvertFrom-Base64URLToBase64
**Description**</br>
  Converts a Base64Url string, to a .Net base64 string.               

**Parameters**
- **Item** - A base64url string

**Examples**
```PowerShell
    ConvertFrom-Base64URLToBase64 `
        -Item "VGhpcyBpcyBhIHRlc3Q"
```

### ConvertFrom-Base64URL
**Description**</br>
Converts a Base64Url string, to a decoded ASCII string.               

**Parameters**
- **Item** - A base64url string Bitbucket Account

**Examples**
```PowerShell
    ConvertFrom-Base64URL `
        -Item "VGhpcyBpcyBhIHRlc3Q"
```
### ConvertTo-JavaTime
**Description**</br>
  Converts the [datetime] object passed into a java compliant numerical representation. (milliseconds since 1/1/1970).               

**Parameters**
- **DateTimeObject** - A DateTime to be converted

**Examples**
```PowerShell
    ConvertTo-JavaTime `
        -DateTimeObject ([DateTime]::Now)
```

### ConvertFrom-JavaTime
**Description**</br>
Converts the java compliant numerical representation of time to a .net [datetime] object.              

**Parameters**
- **Item** - A base64url string Bitbucket Account

**Examples**
```PowerShell
    ConvertFrom-Base64URL `
        -Item "VGhpcyBpcyBhIHRlc3Q"
```
### ConvertTo-JavaTime
**Description**</br>
  Converts the [datetime] object passed into a java compliant numerical representation. (milliseconds since 1/1/1970).               

**Parameters**
- **JavaTime** - A JavaTime to be converted

**Examples**
```PowerShell
    ConvertFrom-JavaTime `
        -JavaTime 1500000000000
```

### ConvertFrom-JavaTime
**Description**</br>
Converts the java compliant numerical representation of time to a .net [datetime] object.              

**Parameters**
- **Item** - A base64url string Bitbucket Account

**Examples**
```PowerShell
    ConvertFrom-Base64URL `
        -Item "VGhpcyBpcyBhIHRlc3Q"
```
### New-ClientInformation
**Description**</br>
  Creates an object to contain client information such as service account details.              

**Parameters**
- **APIKey** - Your service account's APIkey/ClientID as returned by https://console.adobe.io/
- **OrganizationID** - Your OrganizationID as returned by https://console.adobe.io/
- **ClientSecret** - Your service account's ClientSecret  as returned by https://console.adobe.io/
- **TechnicalAccountID** - Your service account's TechnicalAccountID  as returned by https://console.adobe.io/
- **TechnicalAccountEmail** - Your service account's TechnicalAccountEmail  as returned by https://console.adobe.io/

**Outputs**</br>
ClientInformation object to be passed to further commands

**Examples**
```PowerShell
    New-ClientInformation `
        -APIKey "1111111111111222222333" `
        -OrganizationID "22222222222222@AdobeOrg" `
        -ClientSecret "xxxx-xxxx-xxxx-xxxx" `
        -TechnicalAccountID "abcdf@techacct.adobe.com" `
        -TechnicalAccountEmail "xxxx-xxxx-xxxx-xxxx@techacct.adobe.com"
```

### Get-AdobeAuthToken
**Description**</br>
  Adds an adobe auth token to the ClientInformation object passed to it

**Parameters**
- **ClientInformation** - Your ClientInformation object
- **SignatureCert** - Your OrganizationID as returned by https://console.adobe.io/
- **ClientSecret** - The cert that is attached to the specified account. Must have private key. Check which cert at https://console.adobe.io/
- **AuthTokenURI** - URI of the Adobe Auth Service. Defaults to https://ims-na1.adobelogin.com/ims/exchange/jwt/
- **ExpirationInHours** - When the request token should expire in hours. Defaults to 1

**Outputs**</br>
Attached auth token to ClientInformation.Token

**Notes**</br>
Create JWT
 - https://www.adobe.io/apis/cloudplatform/console/authentication/createjwt/jwt_nodeJS.html
 - https://github.com/lambtron/nextbus/blob/master/node_modules/jwt-simple/lib/jwt.js
 - https://jwt.io/

**Examples**
```PowerShell
    Get-AdobeAuthToken `
        -ClientInformation $MyClient `
        -SignatureCert $Cert `
        -ExpirationInHours 12
```

### Get-AdobeUsers
**Description**</br>
  Gets all users from the adobe API

**Parameters**
- **ClientInformation** - Your ClientInformation object
- **UM_Server** - The adobe user management uri. Defaults to "https://usermanagement.adobe.io/v2/usermanagement/"

**Notes**</br>
 - https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplequery.html

**Examples**
```PowerShell
    Get-AdobeUsers `
        -ClientInformation $MyClient
```

### Get-AdobeGroups
**Description**</br>
  Grab a list of all groups, or if provided an ID, returns the group related to the ID

**Parameters**
- **ClientInformation** - Your ClientInformation object
- **UM_Server** - The adobe user management uri. Defaults to "https://usermanagement.adobe.io/v2/usermanagement/"
- **GroupID** - If you wish to query for a single group instead, put the group ID here

**Notes**</br>
 - https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplequery.html

**Examples**
```PowerShell
    Get-AdobeGroups `
        -ClientInformation $MyClient
```

### Get-AdobeGroups
**Description**</br>
  Grab a list of all groups, or if provided an ID, returns the group related to the ID

**Parameters**
- **ClientInformation** - Your ClientInformation object
- **UM_Server** - The adobe user management uri. Defaults to "https://usermanagement.adobe.io/v2/usermanagement/"
- **GroupID** - If you wish to query for a single group instead, put the group ID here

**Notes**</br>
 - https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplequery.html

**Examples**
```PowerShell
    Get-AdobeGroups `
        -ClientInformation $MyClient
    
    Get-AdobeGroups `
        -ClientInformation $MyClient `
        -GroupID "222242"
```

### Get-AdobeGroupMembers
**Description**</br>
  Grab all members of the specified group

**Parameters**
- **ClientInformation** - Your ClientInformation object
- **UM_Server** - The adobe user management uri. Defaults to "https://usermanagement.adobe.io/v2/usermanagement/"
- **GroupID** - The ID of the group to query

**Notes**</br>
 - https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplequery.html

**Examples**
```PowerShell
    Get-AdobeGroupMembers `
        -ClientInformation $MyClient    
        -GroupID "222424"
```

### Get-AdobeGroupAdmins
**Description**</br>
  Grab all admins of the specified group

**Parameters**
- **ClientInformation** - Your ClientInformation object
- **UM_Server** - The adobe user management uri. Defaults to "https://usermanagement.adobe.io/v2/usermanagement/"
- **GroupID** - The ID of the group to query

**Notes**</br>
 - https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplequery.html

**Examples**
```PowerShell
    Get-AdobeGroupAdmins `
        -ClientInformation $MyClient    
        -GroupID "222424"
```

### New-CreateUserRequest
**Description**</br>
  Creates a "CreateUserRequest" object. This object can then be converted to JSON and sent to create a new user

**Parameters**
- **FirstName** - User's First name
- **LastName** - User's Last Name
- **Email** - User's Email and ID
- **Country** -  Defaults to AU. This cannot be changed later. (Per adobe documentation)
- **AdditionalActions** -  An array of additional actions to add to the request. (Like add to group)

**Notes**</br>
 See:</br>
 https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplemultiaction.html
 
 This should be posted to:</br>
 https://usermanagement.adobe.io/v2/usermanagement/action/{myOrgID}

**Examples**
```PowerShell
    New-CreateUserRequest `
        -FirstName "John"   
        -LastName "Doe"     
        -Email "John.Doe@domain.com"
    
    New-CreateUserRequest `
        -FirstName "John"   
        -LastName "Doe"     
        -Email "John.Doe@domain.com"
        -Country "NZ"
```

### New-RemoveUserRequest
**Description**</br>
  Creates a "RemoveUserRequest" object. This object can then be converted to JSON and sent to remove a user from adobe

**Parameters**
- **UserName** - User's ID, usually e-mail
- **AdditionalActions** -  An array of additional actions to add to the request. (Like add to group)

**Notes**</br>
 See:</br>
 https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplemultiaction.html
 
 This should be posted to:</br>
 https://usermanagement.adobe.io/v2/usermanagement/action/{myOrgID}

**Examples**
```PowerShell
    New-RemoveUserRequest `
        -UserName "john.doe@domain.com"
```

### New-UpdateUserRequest
**Description**</br>
  Creates a "UpdateUserRequest" object. This object can then be converted to JSON and sent to update an existing user

**Parameters**
- **UserName** - User's ID, usually e-mail
- **AdditionalActions** -  An array of additional actions to add to the request. (Like add to group)

**Notes**</br>
 See:</br>
 https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplemultiaction.html
 
 This should be posted to:</br>
 https://usermanagement.adobe.io/v2/usermanagement/action/{myOrgID}

**Examples**
```PowerShell
    New-UpdateUserRequest `
        -UserName "john.doe@domain.com"
```

### New-RemoveUserFromGroupRequest
**Description**</br>
  Creates a request to remove a user from an Adobe group. This will need to be posted after being converted to a JSON

**Parameters**
- **UserName** - User's ID, usually e-mail
- **GroupName** - User's ID, usually e-mail

**Notes**</br>
 See:</br>
 https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplemultiaction.html
 
 This should be posted to:</br>
 https://usermanagement.adobe.io/v2/usermanagement/action/{myOrgID}

**Examples**
```PowerShell
    New-RemoveUserFromGroupRequest `
        -UserName "john.doe@domain.com" `
        -GroupName "My User Group"
```

### New-GroupUserAddAction
**Description**</br>
  Creates a "Add to group" action. Actions fall under requests. This will have to be added to a request, then json'd and submitted to adobe's API

**Parameters**
- **Groups** - An array of groups that something should be added to

**Notes**</br>
 See:</br>
 https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplemultiaction.html
 
 This should be posted to:</br>
 https://usermanagement.adobe.io/v2/usermanagement/action/{myOrgID}

**Examples**
```PowerShell
    New-GroupUserAddAction `
        -Groups "My User Group"
```

### New-GroupUserRemoveAction
**Description**</br>
  Creates a "Remove from group" action. Actions fall under requests. This will have to be added to a request, then json'd and submitted to adobe's API

**Parameters**
- **Groups** - An array of groups that something should be added to

**Notes**</br>
 See:</br>
 https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplemultiaction.html
 
 This should be posted to:</br>
 https://usermanagement.adobe.io/v2/usermanagement/action/{myOrgID}

**Examples**
```PowerShell
    New-GroupUserRemoveAction `
        -Groups "My User Group"
```

### New-AddToGroupRequest
**Description**</br>
  Creates a "Add user to group" request. This will need to be json'd and sent to adobe

**Parameters**
- **Groups** - An array of groups that something should be added to
- **User** - A User to be added to the Group(s)

**Notes**</br>
 See:</br>
 https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplemultiaction.html
 
 This should be posted to:</br>
 https://usermanagement.adobe.io/v2/usermanagement/action/{myOrgID}

**Examples**
```PowerShell
    New-AddToGroupRequest `
        -Groups "My User Group"     
        -User "John.Doe@domain.com"
```

### New-RemoveFromGroupRequest
**Description**</br>
  Creates a "Remove user from group" request. This will need to be json'd and sent to adobe

**Parameters**
- **Groups** - An array of groups that something should be removed from
- **User** - A User to be removed from the Group(s)

**Notes**</br>
 See:</br>
 https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplemultiaction.html
 
 This should be posted to:</br>
 https://usermanagement.adobe.io/v2/usermanagement/action/{myOrgID}

**Examples**
```PowerShell
    New-RemoveFromGroupRequest `
        -Groups "My User Group"     
        -User "John.Doe@domain.com"
```

### Expand-JWTInformation
**Description**</br>
   Unpacks a JWT object into it's header, and body components. (Human readable format)

**Parameters**
- **JWTObject** - JWT To unpack. In format of {Base64Header}.{Base64Body}.{Base64Signature}
- **SigningCert** - A certificate with the necesary public key to verify signature block. Can be null, will not validate signature.

**Notes**</br>
 See:</br>
 https://www.adobe.io/apis/cloudplatform/usermanagement/docs/samples/samplemultiaction.html
 
 This should be posted to:</br>
 https://usermanagement.adobe.io/v2/usermanagement/action/{myOrgID}

**Examples**
```PowerShell
    Expand-JWTInformation `
        -JWTObject "xxxx.xxxx.xxx"
```

### Send-UserManagementRequest
**Description**</br>
   Sends a request, or array of requests, to adobe's user management endpoint

**Parameters**
- **ClientInformation** - ClientInformation object containing service account info and token
- **Requests** - An array of requests to send to Adobe.

**Notes**</br>
 See:</br>
 Create-*Request

**Examples**
```PowerShell
    Send-UserManagementRequest `
        -ClientInformation $MyClientInfo `
        -Requests ( `
            New-CreateUserRequest `
                -FirstName "John" `
                -LastName "Doe" `
                -Email "john.doe@domain.com" `
                -Country="AU" `
        )
```

### New-SyncADGroupRequest
**Description**</br>
   Creates an array of requests that, when considered together, ensures an Adobe group will mirror an Azure AD group

**Parameters**
- **ADGroupID** - Azure AD Group Identifier. The source group to mirror to Adobe
- **AdobeGroupID** - Adobe group ID as retured from Get-AdobeGroups
- **ClientInformation** - Service account information including token

**Examples**
```PowerShell
    New-SyncADGroupRequest `
        -ADGroupID "SG-My-Approved-Adobe-Users" `    
        -AdobeGroupID "111222422" `
        -ClientInformation $MyClientInfo
```

### New-RemoveUnusedAbobeUsersRequest
**Description**</br>
   Creates an array of requests that, when considered together, removes all users that are not admins, and not part of any user groups

**Parameters**
- **ClientInformation** - Service account information including token

**Examples**
```PowerShell
    New-RemoveUnusedAbobeUsersRequest `
        -ClientInformation $MyClientInfo
```

## Prerequisites
- Azure Tenant 
- Azure Automation Account

## Versioning
[Github](http://github.com/) for version control.

## Authors
* **Paul Towler** - *Initial work* - [AdobeUM](https://github.com/mrptsai/AdobeUM)

See also the list of [contributors](https://github.com/mrptsai/AdobeUM/graphs/contributors) who participated in this project.
