#Github_Logger Project#

*****TESTING*****

This project is able to monitor when PR was merged to the given repo, parsing the information
that being accepted by the GIT API, and presenting the information regarding the repo and the 
files that were changed/added/deleted by the specific PR.

The abovementioned process is achieved by a dedicated Python code, running by a dedicated Lambda function
on AWS.

To use this project, make sure to meet the following prerequisites:
To use this project, you should make sure to have the following:

1. AWS Access Key / Secret Key available.
2. GitHub Token with the appropriate permissions to modify files/update repos, create and merge PR, etc.
3. Terraform Client that will create the project.


#Cost:#
We are using the following AWS services:

--- Lambda ---  Very low cost, calculated based on the Run-Time and invocation number.
Our project's run time is very short(avg of 113.013 ms).

--- CloudWatch --- Considering that we don't save anything else besides the logs(we are not cloning the repo) 
the cost of the CloudWatch in our region(eu-west-3/Paris) is $0.5985 per GB.

#Load:#

The queries that we are sending are very short(as mentioned above), and thus we do not having a high load
for our services.
In addition, the payload we are getting from the API-Request is small, which is another fact regarding
the minimum required load.

#Security:#

The only security Key we are using in our project(considering the user already configured his ENV VARs with the AWS Keys)
is the GIT_Token, which is not hard coded in our resources.
The user inserts the Token the first time we run 'terraform apply', and then, the Token is saved
under an ENV variable that is being referenced in the 'main.py'.

An explanation regarding each file in our project:

#'main.tf':#

This is the main file of our project, which contains all the resources we are configuring in our AWS Cloud provider.
For example: aws_iam_role related resources, lambda_function, etc.

#'main.py'#

This is the file that contains the Python code that is being run by the Lambda function once triggered.
Using this code, we are able to get the required information once there is a PR that was merged
to the given repo by parsing the API payload.
The API payload was taken from GIT_Docs, and the VARs we are using inside the API request are based on:
--Parsed API Payload using JSON Formatting.
--ENV vars that were added to the Lambda function.

#'github.tf':#

This is the file that contains all the git-related resources we are configuring in our project.
For example, github_repository, github_webhook, etc.

#'vars.tf':#

This file contains all the vars we are using in our project and their values.
Please note that before running the 'terraform apply' for the first time, some of the values 
will be empty, as they need to be filled by the User's input.
For example, github_token

#'output.tf':#

This file contains the values that will be printed after using 'terraform apply/plan' for an easier/faster reference 
for the user.
