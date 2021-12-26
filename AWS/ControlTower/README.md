# Use Ermetic with AWS Control Tower

Ermetic integration with Control Tower - Allow new or updated AWS accounts in a AWS Control Tower based AWS Organization to automatically deploy an Ermetic integration IAM role and register with Ermetic.

## How it Works

 1. Template: [**ermetic-controltower.yaml**](cloudformation/ermetic-controltower.yaml)
     - This template provisions infrastructure in the Control Tower management account that allows creation of an Ermetic integration IAM role in Control Tower managed accounts whenever a new Control Tower managed account is added.
     - Creates an Ermetic stack set in the Control Tower management account.
     - Provisions a CloudWatch Events rule that is triggered based on Control Tower lifecycle events.
     - Provisions a Lambda as a target for the CloudWatch Events rule.
        - The Lambda deploys an Ermetic integration IAM role in the newly added Control Tower managed account, thus enabling Ermetic to access the managed account.

## Solution Design

<img src="media/image1.png" />

## How to Deploy and Test

 1. **Deploy - AWS Control Tower Management Account**
     - Launch the [**ermetic-controltower.yaml**](cloudformation/ermetic-controltower.yaml) template in the AWS Control Tower management account.
        - Ensure that a CloudFormation stack set is successfully created.
        - Ensure that a CloudWatch Events rule is successfully created with an Lambda target to handle Control Tower lifecycle events.
 2. **Test - Add a Managed Account** 
     - From the Control Tower management account:
        - Use Account Factory or quick provision or Service Catalog to create a new managed account in the Control Tower organization or
        - Use Service Catalog to update an existing managed account - e.g. change the OU of an existing managed account.
        - This can take up to 30 mins for the managed account to be sucessfully created and the Control Tower lifecycle event to trigger.
        - Login to the Control Tower managed account - 
           - Validate that a CloudFormation stack has been provisioned and that the Ermetic integration IAM role has been created.
 3. **Validate**
     - Sign in to your Ermetic account and validate that identity intelligence data for the new account is being collected and displayed on the Ermetic dashboard
