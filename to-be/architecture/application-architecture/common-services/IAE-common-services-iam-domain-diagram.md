![Image of the IAM Domain Diagram](../../images/IAE-common-services-iam-domain-diagram.png)
##IAM Domain Diagram (Interpretations)
###Business Class Modeler: Topic: IAM Domain Diagram
* Created: 4/4/2014 1:07:13 PM
* Changed: 4/4/2014 1:37:44 PM by Local User:PamelaAMiller (Owner of local repository)
* Version: 0
* Status: Draft
###Authenticated Identity
Type: Concrete
For each Authenticated Identity:
 we will find a Token it generates
###Authentication Entry Point
The location that the service is accessed by the Clients.
Type: Concrete
For each Authentication Entry Point:
 * we will find an Identity it is associated with
Credential
The combination of the identity and factor being authentication.
Type: Concrete
It includes two constituent Business Classes:
 Factor
A part of a credential that is unique to the requester seeking authentication. A valid factor in a 
two-factor authentication system is some combination of Have and Know.
 Identity
The unique representation of the requestor for identification that is grouped by 1 or more Roles
Each Credential can perform:
 Send Credential
The combination of the identity and factor that is used for authentication.
 Check Credential
Element
(a kind of Have)
A generic instance of a second factor “Have”
Type: Concrete
Expiration
A timeout for the token. If a token is expired it is invalid. The configuration of timeouts can be 
constrained by role.
Type: Concrete
It is a constituent of:
 Token
The representation of the authentication transaction. Valid by a constrained Expiration. This is  
checked by services to determine if identity has been confirmed and authenticated. This is 
proved by a valid token from IA&M. The individual service will determine access.
Factor
A part of a credential that is unique to the requester seeking authentication. A valid factor in a two-factor 
authentication system is some combination of Have and Know.
Type: Concrete
It is a constituent of:
 Credential
The combination of the identity and factor being authentication.
Have
(a kind of Factor)
An authentication factor which relies on the users possession of a particular, physical item that is not part 
of their body, e.g., a PIV card.
Type: Concrete
IAM Manage Entry Point
The location that the service is accessed by the user or administrator.
Type: Concrete
For each IAM Manage Entry Point:
 we will find an Identity Manager it is associated with
Identity
The unique representation of the requestor for identification that is grouped by 1 or more Roles
Type: Concrete
It is a constituent of:
 Credential
The combination of the identity and factor being authentication.
 Role
The hierarchy of roles as defined by business hierarchy not defined by system usage roles.
I&AM is not a federated access to resource system. It is an augmented credentialing system. A 
global (external List to system) Role is augmented to iden

