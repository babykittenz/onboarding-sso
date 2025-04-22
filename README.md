Microsoft Active Directory to AWS Cognito Integration Guide
This guide outlines the responsibilities for both parties to successfully integrate Microsoft Active Directory with AWS Cognito for single sign-on functionality.
What YOU Need to Do (Application Owner)
1. Prepare Your AWS Cognito Environment

Create a Cognito User Pool (if not already done)
Set up an App Client within the User Pool
Configure callback URLs for your application
Enable SAML as an authentication provider

2. Provide the IT Department with These Details

Your Cognito User Pool ID
Your Cognito domain URL (https://[your-cognito-domain].auth.[region].amazoncognito.com)
The SAML assertion consumer service URL (https://[your-cognito-domain].auth.[region].amazoncognito.com/saml2/idpresponse)
The audience URI (urn:amazon:cognito:sp:[your-user-pool-id])
List of user attributes you need from Active Directory (typically email, first name, last name, groups)

3. After Receiving IT Department Information

Configure the SAML Identity Provider in your Cognito User Pool

Upload the federation metadata XML file they provide
Map the AD attributes to Cognito attributes


Set up the App Client to use the SAML provider
Test the integration with credentials provided by IT

4. Final Steps

Add the company's identity provider to your app client's allowed providers
Configure attribute mapping to match what the IT department is sending
Set up group mapping if using AD groups for permissions

What the IT Department Needs to Provide
1. Required Information

Their AD FS Federation Metadata XML file

Usually available at: https://[their-adfs-server]/FederationMetadata/2007-06/FederationMetadata.xml


Their AD FS Entity ID (usually the same as the metadata URL)
List of attribute names they will be passing (must include email at minimum)
Test user credentials for verification

2. Actions They Need to Perform
Step 1: Set Up SAML 2.0 Identity Provider in AD FS

Log in to the AD FS management console

Open the AD FS Management console on the AD FS server
Navigate to "Trust Relationships" > "Relying Party Trusts"


Add a new Relying Party Trust

Click "Add Relying Party Trust" in the Actions pane
Choose "Enter data about the relying party manually"
Provide the following details (which you will supply):

Display name: [Your Application Name] Authentication
Configure URL: https://[your-cognito-domain].auth.[region].amazoncognito.com/saml2/idpresponse
Relying party trust identifier: urn:amazon:cognito:sp:[your-user-pool-id]




Configure Claim Rules

Add rules to send the following LDAP attributes as claims:

Email address (required)
Given name (recommended)
Surname (recommended)
Groups (if using AD groups for authorization)


Format the NameID as an email address


Export Federation Metadata

Provide the federation metadata XML file to you
Typically found at: https://[their-adfs-server]/FederationMetadata/2007-06/FederationMetadata.xml



Step 3: Test the Integration

Initial Testing

Provide test user credentials
Verify the user can authenticate through the Cognito hosted UI


Troubleshooting Common Issues (if needed)

Check AD FS diagnostic logs
Verify claim rules are correctly configured
Ensure certificates are valid and trusted



Additional Technical Considerations for the IT Department

Ensure that communication between AD FS and AWS Cognito is encrypted using TLS
Consider certificate expiration dates (SAML certificates typically valid for 1-3 years)
Network connectivity requirements between their environment and AWS
Firewall configurations to allow traffic between AD FS and Cognito

Support Contact
If the IT department encounters any issues during this process, they should contact:

Michael Kidby
mike@q4impact.com
