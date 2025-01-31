## Challenge 01 Solutions: 
***
#### Enabling Console Access 
1) Navigate to the IAM Console and select Users from the menu on the left side. Find the SarahJaneSmith user. Click on the user and then find the "Security Credentials" 

2) Click the "Enable console access" under the "Console sign-in" section. 
<img width="857" alt="step2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/dc487a38-001a-4148-8158-8345d8baca30">

3) Click the "Enable" button. For the password, choose a custom password. Give the user a password. Make sure to remember it.
<img width="460" alt="step3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/294e93c8-69ef-4f31-9ddd-dd40f4bbd567">

4) Be sure the checkbox for user must create new password at next sign-in is DISABLED. Click apply. Download the password csv if you need it. 
<img width="465" alt="step4" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/bae7f086-b297-4ecf-8cac-bf42f1f48608">

5) Copy the console sign-in link into a text editor of your choice. 
<img width="637" alt="step5" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/4210e8b6-6baf-4834-9e02-0902ab937b81">

6) Using a different browser, or a private browser, use the link for the console sign-in. Enter the IAM user name and the password you created. The account number should be auto-filled. 
<img width="959" alt="step6" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/5eba7100-ec0f-49fe-a119-a4ade40a8141">

7) Login and check out the EC2 instances. You should be able to see them. (**Note: Make sure you're in the same region as the Admin user**, otherwise you will be unable to view the instances.)
<img width="960" alt="step7" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/2e70d771-ff1c-4012-96da-06d04bf8a356">

8) Attempt to reboot one of the instances. What happens? 
<img width="793" alt="step8" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/ad0b21d6-32c4-4572-b06b-0c557d41e710">
<img width="794" alt="step8_2nd" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/ceec7425-f645-4254-9525-aec9169ea69c">

9) You can leave this window open for the time being. 

### Creating a Role
1) Go back to the Admin user where you created the CloudFormation Stack. 

2) Navigate to IAM console and select "Roles" from the menu on the left side. Click "Create Role" on the top, right-hand side.
<img width="911" alt="2_step2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/51dfd319-2aa1-4de0-b3e0-1f320a6937aa">

3) Under Trusted Entity type, select AWS Account. Be sure that "This account" is selected below. Click next. 
<img width="788" alt="2_step3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/7d167c59-6aab-4ce6-b357-8668cb497715">

4) Search for "EC2" in the search bar. Find and select checkbox the `AmazonEC2FullAccess` policy. 
<img width="676" alt="2_step4" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/be5f1472-2347-4d9a-819b-a61d804dc4c1">

5) Clear the search bar by click the X next to EC2. Search for S3. Find and select the checkbox for `AmazonS3FullAccess`. Scroll down and click next. 
<img width="638" alt="2_step5" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/f2aa6fa9-0fdb-4f1a-b42e-0adedc56b781">

6) Give the role a name called `EC2_S3_FullAccess`
<img width="508" alt="2_step6" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/089362a1-bc3a-47ec-8873-934848251877">

7) Review the JSON policy so you understand what's happening here. This policy is allowing anyone within the AWS Account that you selected to assume this role and have the permissions associated with this role. 
<img width="640" alt="2_step7" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/cf41acbc-dee7-4217-a212-3b001aab8e64">

8) Click Create Role and make sure it completes before moving on. Click on the Role and copy the ARN. 
<img width="674" alt="2_step8" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/65a9c277-3d2e-46f6-a516-6477729762b2">


### Assuming a Role
1) Move back to the window where the `SarahJaneSmith` user has logged in. 

2) In the top right hand menu, where your user name `SarahJaneSmith` is located, click the dropdown. Copy down the Account ID. 
<img width="246" alt="3_step2" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/499fb6c3-3c32-4172-9f8e-00957f1c2771">

3) Towards the bottom of the menu, click the "Switch Role" button. 

4) Click the blue Switch Role button. 

5) Enter the Account ID, the Role name (`EC2_S3_FullAccess`) and pick a display name (`EC2_S3_Admin`) and then click Switch Role. 
<img width="796" alt="3_step5" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/d8d6fdc9-f9ab-4b40-8af9-e9d71ebf0161">

6) This doesn't work. Why? Well, the user has permissions for EC2 and S3 ReadOnly, but does it have permissions to use STS and assume a role? We need to modify the permissions of the SarahJaneSmith user to allow this. 

7) Keep the window open, but move back to the Admin user window. 

8) Move to IAM and the SarahJaneSmith user. Under permissions, click the drop down for Add Permissions. Select "Add inline policy" 
<img width="650" alt="3_step8" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/4c42e7ba-1db2-4bc6-a588-cef9b45303b6">

9) Under service, search and choose "STS" 

10) Expand the "Write" selection and check the `AssumeRole` box. 
<img width="665" alt="3_step10" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/da15f130-ccd8-4cd8-a014-7fd98657a179">

11) For Resources, choose "All resources" Click Next. 

12) Name the policy `STS-Assume-Role`. Click Create Policy. 

13) Verify that the policy is added under Permissions policies. 
<img width="671" alt="3_step13" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/ae79f202-1ad1-4688-9416-bab372b0aa17">

14) Attempt Steps 1 through 4 again. It should work this time. 
<img width="960" alt="Screenshot 2023-08-03 123807" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/995e90db-b024-4de3-a7d9-bb46eca35dc9">

15) Verify that the role is active by rebooting any of the EC2 instances. 
<img width="960" alt="Screenshot 2023-08-03 123925" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/f5fd183a-39b4-4245-a32e-f6b890854e78">

## Challenge 02 Solutions: 
***
1) Navigate to the S3 Console and select the bucket that was created for you. Click on "Properties" and copy the ARN of the bucket. 
<img width="645" alt="c2_step1" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/5a0090bc-cb63-433d-80e3-7ee8e091f83a">

2) Navigate to the AWS Policy Generator. https://awspolicygen.s3.amazonaws.com/policygen.html

3) For the type of policy, choose "S3 Bucket Policy" 

4) Enter the following for the Statement: 
    - Principal = "*"
    - AWS Service = Amazon S3
    - Actions = s3:GetObject
           <img width="784" alt="Screenshot 2023-08-03 125234" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/88a645a7-dbd7-4f17-b8d7-24d2f26ccded">

    - For the ARN you will want to copy the ARN of the bucket and then add a backslash and star. This makes sure that the objects in the bucket (rather than the bucket itself) are the targets. E.g., `arn:aws:s3:::week-04-bucketname/*`
          <img width="646" alt="Screenshot 2023-08-03 125336" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/ad7bc9f7-b2e4-427a-82d6-85a26ad8ce96">

    - Next we will add conditions. Since the customer only wants objects that are tagged with a key-value pair of `environment:aws-security-labs` we will have to make sure that this is accounted for in the bucket policy. Click the "Add Conditions" link. 
        - We will need to make sure that we're adding a "StringEquals" condition, since we're looking for an exact match of a tag. 
        - Since we are looking for an existing object tag, find and select the key that matches that condition. This should be s3:ExistingObjectTag/<key>
        - Then for value, add `aws-security-labs`
              <img width="765" alt="Screenshot 2023-08-03 125429" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/df8ae08a-3fd0-4adc-b18b-786b8725ee10">

        - Hit the "Add condition" button.
              <img width="748" alt="Screenshot 2023-08-03 125452" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/8c40c20a-53c5-4bb1-b3b8-6dd017e64146">

    - Now you can hit the "Add Statement" to generate the statement to the policy.
          <img width="788" alt="Screenshot 2023-08-03 125512" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/64c497bf-2d2c-48d3-8e2a-d0d84a72b5ca">

    - We will have to do some modification, but you can hit the 'Generate Policy' to get the policy created. 
        <img width="616" alt="Screenshot 2023-08-03 125532" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/abb1410a-2a63-442e-a5fe-5d2a091b5a01">


5) Copy the policy JSON and then navigate back to the bucket. Select "Permissions" 

6) Under Bucket policy, select the edit button to edit the policy. 
<img width="902" alt="Screenshot 2023-08-03 125922" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/28a590f9-5596-4fde-84ec-43c87e38c699">

7) Paste the policy into the text field.

8) Here is where we'll need to modify the policy slightly. On line 14, where the policy states `<key>`, we'll need to change this to the key that is in the customer requirements. Be mindful of capitialization, because tags are case sensitive. 
<img width="391" alt="Screenshot 2023-08-03 130915" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/cdcb4cef-0759-48e0-814d-47d67914dc58">

9) Scroll down and hit "Save changes"

10) Upload the `index.html` file into the bucket. Upload the `index2.html` file into the bucket. Do not add any tags. Once this is complete, in the properties section of the object you will see an "Object URL". Right click and open in a new tab or window. Repeat this for both files. 

11) You will get an error. This is because of the policy that we added to the bucket. Objects can't be public unless they meet a specific tag requirement. Let's add that tag now. Leave the window or tab open. Go back to the S3 console and go to the properties of the `index.html` object. Scroll down to the Tags section. Add the key-value pair that the customer requires. Click "Save Changes". 

12) Now, go back to window or tab that you opened for the object and refresh the tab or window. Now the object is accessible because the tag matches the policy evaluation. Do the same for `index2.html` and verify that you still don't have access to that file. 


## Challenge 03 Solutions: 
***
## Enable Console Access
1) Navigate to the IAM Console and select Users from the menu on the left side. Find the AmeliaPond user. Click on the user and then find the "Security Credentials"

2) Click the "Enable console access" under the "Console sign-in" section. 
<img width="706" alt="Screenshot 2023-08-03 133950" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/e3792bf3-901f-4476-892f-6bea10f393d8">

3) Click the "Enable" button. For the password, choose a custom password. Give the user a password. Make sure to remember it.
<img width="412" alt="Screenshot 2023-08-03 134137" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/fd862463-f5d1-4899-9e50-932b20621f12">

4) Be sure the checkbox for user must create new password at next sign-in is DISABLED. Click apply. Download the password csv if you need it. 

5) Copy the console sign-in link into a text editor of your choice. 
<img width="412" alt="Screenshot 2023-08-03 134152" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/8a93394c-6074-4eb7-a118-00ecb4a03cae">

6) Using a different browser, or a private browser, use the link for the console sign-in. Enter the IAM user name and the password you created. The account number should be auto-filled. 
<img width="960" alt="Screenshot 2023-08-03 134239" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/c98219f1-8eaf-414d-9de7-28fb78268322">

7) Login and check out the EC2 instances. You shouldn't be able to see them. 
<img width="960" alt="Screenshot 2023-08-03 134254" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/eeef04b1-bded-4779-8f48-4be41c8c6365">

8) You can leave this window open for the time being. 

## Creating a Dynamic Policy 
***
10) Navigate to the AWS Policy Generator. https://awspolicygen.s3.amazonaws.com/policygen.html

11) We're going to select IAM Policy and this will be for EC2 Service. For actions, find and check the actions for RebootInstance, StartInstance, StopInstance. 

12) This should be for all instances ("*"), add this into the ARN field. Next we'll add conditions. 
<img width="749" alt="policy2_3" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/e00953d5-4527-46c3-a400-31beda963524">

13) Click conditions. Select StringEquals for the condition and `ec2:ResourceTag/${TagKey}` as the key. We're going to add a placeholder of `ec2-tag-value` here as we'll need to make this statement dynamic in nature. Click Add Condition. 
<img width="777" alt="policy2_4" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/2bb1f5b0-06e9-4d4d-a86d-d1839ab7fde3">

14) Add another condition, but this time, use the `aws:PrincipalTag/${TagKey}` key. Use `user-value` as the value. This is just a placeholder and will be replaced. Click "Add Statement. 
<img width="775" alt="policy2_5" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/481c3d4b-c496-4435-ab2a-14895d9bd04a">

15) Let's add another statement. This time you should select EC2 as the service, and then under actions, find and select the following: 
    - DescribeAvailabilityZones
    - DescribeInstanceStatus
    - DescribeInstanceTypes
    - DescribeInstances
<img width="736" alt="policy2_step15" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/d3dc33b8-dc44-40ef-98b5-0d34028d5d5b">

16) Use "*" in the ARN field and then click Add Statement. Review the statements, and if you're done, click "Generate Policy" 
<img width="752" alt="policy2_step16" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/5e86256f-6423-4ba5-86a8-393c64eb56e6">

17) Copy this into a text editor and then navigate to the IAM Console and create a new policy. Use the JSON Editor and replace the pre-generated text with the JSON policy you just generated. 
<img width="615" alt="policy2_step17" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/af513c6a-9cdd-4210-9688-03442d8f34f7">

18) The problem statement that we needed 1 policy that was applied to all users. This policy had to make sure that users could only interact with the EC2 instances that had the same tag key-value pair as the users. E.g., Only users that have a tag of `Department:Production` should be able to manage the EC2 instances that have a tag of `Department:Production`. They should not be allowed to manage EC2 instances with a tag of `Department:Development` or really anything that wasn't `Department:Production`. So, to accomplish this, we need to use variables instead of hard-coding values. First, like in the second challenge, we're going to replace `TagKey` with the tag key that we want to consolidate on. So in the line that says `"ec2:ResourceTag/${TagKey}"`, replace `${TagKey}` with `Department`. 
<img width="720" alt="Screenshot 2023-08-03 135216" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/98925050-0891-499a-9bf6-ae9456d4000b">
<img width="868" alt="Screenshot 2023-08-03 135247" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/336f53e4-a49f-4fcf-a918-c1a28744265c">
<img width="314" alt="Screenshot 2023-08-03 135355" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/d3253cdd-65f7-413c-a37c-d658d4e6e2a7">
<img width="358" alt="Screenshot 2023-08-03 135428" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/bfc266a5-caa1-4bec-a20d-08088fb3a5c1">

19)  Now this is where the fun begins. Now we need to tell the policy to evaluate the value of the `Department` key for the user attempting to access the resource. For this, we're going to have to add another variable, but this time, in the "Value" portion of the statement. Let's modify the `aws:PrincipalTag/${KeyTag}` to reflect the `Department` value that we're looking for in a user. So like we did for the resource tag, replace `${TagKey}` with `Department`. Now, copy that `"aws:PrincipalTag/Department"` and replace `user-value` in the resource value in that line. So now the `ec2:Resource` line should look like this: `"ec2:ResourceTag/Department": "aws:PrincipalTag/Department"`. Let's add some brackets and dollar sign to the value portion so that the policy knows it's a variable. The final version should look like this: `"ec2:ResourceTag/Department": "${aws:PrincipalTag/Department}"`. Be sure to delete the line that starts with `aws:PrincipalTag`. If you leave this in here, the policy will not work. Click next and give the policy a name of `Dynamic_EC2_Policy`. Click Create. 
<img width="399" alt="Screenshot 2023-08-03 135650" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/be157405-668e-4423-8b8b-92dd78475636">

20) Back in the IAM Console, go to Users -> AmeliaPond. Let's modify this users permissions so that this policy is added. Click Add Permissions -> Add Permissions. Attach a policy directly. Search for the `Dynamic_EC2_Policy` that we just created. Click Next and then Add permissions. 
<img width="701" alt="Screenshot 2023-08-03 140421" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/2de75ce6-733d-4fb3-9619-f4e54fbf708d">

## Verifying Access 
*** 

21) Log back into the console as the `AmeliaPond` user. Navigate to EC2 and you should be able to see all of the instances. Attempt to restart the Production instance. This should work now. Attempt to restart any of the other instances. It shouldn't work because the other instances have different tags than the `AmeliaPond` user. Don't close the window just yet. 
<img width="959" alt="Screenshot 2023-08-03 140802" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/f1a625b6-20c6-44ba-839c-35fe1a4fac88">
<img width="804" alt="Screenshot 2023-08-03 140819" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/1c9b82cb-e29a-4440-a500-78a34e70a2eb">

22) Let's go back to the Admin window. Go to the IAM console --> Users. Click the `AmeliaPond`user. Go to Tags. 
<img width="722" alt="Screenshot 2023-08-03 140854" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/e0074af2-a91b-4465-b39e-ab6a89a60e9e">

23) Click "Manage Tags" Change the value of Department from 'Production' -> 'Application'. Save changes. 
<img width="893" alt="Screenshot 2023-08-03 140952" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/b5551168-be55-4d1c-ad25-476b66062c4f">

24) Go back to the AmeliaPond window and try to restart the Application instance. This should work, but now you won't be able to restart the Production instance. 
<img width="797" alt="Screenshot 2023-08-03 141046" src="https://github.com/rhearora/aws_security_labs_copy/assets/129975163/9b1f51f1-1a8e-49c6-99ef-0ffc928ebc5e">

## Environment Clean Up 
- Before you delete the stack, perform the following: 
- Make sure that the policies are removed from the `AmeliaPond` user. Also make sure that you disable console access. 
- Make sure you remove the `sts-assume-role` policy from the `SarahJaneSmith` user. Also make sure that you disable console access. 
- Delete any roles or policies that you have created. 
- Remove the `index.html` from the S3 bucket. 
- Delete the stack
