# User creation and user attributes.

## Overview  
In this lab, user accounts are created and configured. While creating a user is a simple task, the main focus is on properly configuring user attributes. These attributes define the identity of the user and are important for management, access control, and later synchronization to Microsoft Entra ID.

The lab builds on the previous step, where groups were created for the Trading department. The users created here will later be added to those groups and used to test access to resources.

## Objectives  
- Create user accounts in a structured way
- Configure relevant attributes to support identity management
- Show the importance of user attributes

## Implementation  
#### Step 1: User creation.
Users are created directly in the appropriate Organizational Unit to reflect the structure of the organization: 01_Users -> Aarhus -> Trading

For each user we have to provide: 
- First name
- Last name
- Full name "Is filled out automatically"
- User logon name: The UPN format for our organization is firstName.LastName@klarstroem.local

I'll start out with creating my first user, Mark Nielsen:
![Mark user](screenshots/mark.png)

Next, I chose *User must change password at next logon*, this is a common approch in real organizations. I would then hand the user the temporary password, and then the user had to change the password when trying to logon for the first time. This ensures that no one other than the user knows the password to that account:

![new password](screenshots/password.png)

Here is the final output:
![final output](screenshots/finaloutput.png)

After I had successfully created the user Mark Nielsen, I then went ahead and created an additional user named **Line Hansen**

#### Step 2: Attribute configuration.
After creating the user, additional attributes are configured to enrich the identity of the accounts.

I chose to define the following attributes:

General tab:
- Display name
- Email address

Organization tab:
- Department (Trading)
- Job title

Address tab:
- Country

The attributes are important because they provide context about the user and are used in several scenarios:
- Department can be used for grouping and access control
- Country is a must when assigning licenses in Entra ID
- Email is used for communication and identity in cloud services

The specific attributes that must be filled depend on company policy. In this lab, only a few is configured to demonstrate the concept.

To configure the above mentioned attributes, I simply right clicked on the user "Mark Nielsen" and then clicked properties:

![configuring attributes](screenshots/attributes.png)


## Verification

## Results

## Lessons Learned
