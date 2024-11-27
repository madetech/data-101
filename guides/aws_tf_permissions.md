# AWS TF Permissions

## Guide to Documenting TF Permissions

The following guide is for documenting permissions for Terraform scripts to run. The approach is taken from:

[AWS Permissions TF - StackOverflow](https://stackoverflow.com/questions/51273227/whats-the-most-efficient-way-to-determine-the-minimum-aws-permissions-necessary)

### Enable Detailed Logging:

The following table illustates the different levels of debugging available in Terraform

| Debug Level | Description                                                                                                                                     | Recommended Usage                                                  |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| **TRACE**   | Provides the most detailed level of logging, capturing every function call and variable change within Terraform. Generates a very large output. | Use for deep debugging to troubleshoot complex issues.             |
| **DEBUG**   | Logs each execution step, including API requests/responses and error messages. Offers comprehensive insights into Terraform's actions.          | Useful for investigating configuration issues or API interactions. |
| **INFO**    | Shows significant actions like resource creation, updates, and deletions without overwhelming details.                                          | Ideal for observing Terraform’s behaviour in a balanced way.       |
| **WARN**    | Logs potential issues or warnings that may not stop execution but could affect performance or future runs.                                      | Good for monitoring and catching early warnings.                   |
| **ERROR**   | Logs only critical errors without additional operational details.                                                                               | Best for production environments to avoid verbose output.          |


Set `TF_LOG=DEBUG` in your environment variables. This enables very detailed logging for Terraform, including information on AWS API calls made by Terraform during execution.

```bash
export TF_LOG=DEBUG
export TF_LOG_PATH="terraform.log"
```

Run the Terraform Plan:

Execute terraform plan or terraform apply with the TF_LOG=TRACE setting enabled. This will generate a log file with all the API calls Terraform is making.
```bash
terraform apply
```

### Analyse the Logs:

Review the logs to identify each AWS API call made by Terraform. Since each call corresponds to an action or data retrieval, this provides insights into the specific AWS permissions required.

Run the following commands to get at the rpc calls (remote procedure calls, should be our api calls to AWS)

```bash
grep "HTTP Request" "<path_to_log_file>" | grep -E "rpc.method|rpc.service" > "<path_to_rpc_log_file>"
```

1. grep "HTTP Request" "<path_to_log_file>":
   + The first grep command searches for lines containing the exact phrase "HTTP Request" in the specified log file (<path_to_log_file>).
   + It outputs all lines that include this phrase, filtering out any lines that do not contain it.
2. | grep -E "rpc.method|rpc.service":
   + The output of the first grep command is passed through a pipe (|) to a second grep command.
   + The second grep uses the -E option to enable extended regular expressions, allowing it to search for either "rpc.method" or "rpc.service" in each line.
   + It matches lines that include either of these terms, filtering out any other lines.
3. \> "<path_to_rpc_log_file>":
   + The final filtered output is redirected using > to save the results to a new file (<path_to_rpc_log_file>).
   + This file will contain only the lines from the original log that include both "HTTP Request" and one of "rpc.method" or "rpc.service".

### Make it Pretty:

The following will remove the any other information that is not the rpc.method or rpc.service.

```bash
awk '
{
    rpc_method = ""; 
    rpc_service = "";
    for (i=1; i<=NF; i++) {
        if ($i ~ /^rpc\.method=/) {
            sub(/^rpc\.method=/, "", $i);
            rpc_method = $i;
        }
        if ($i ~ /^rpc\.service=/) {
            sub(/^rpc\.service=/, "", $i);
            rpc_service = $i;
        }
    }
    # Print rpc.method and rpc.service, leave blank if either is missing
    print "rpc.method=" (rpc_method ? rpc_method : ""), "rpc.service=" (rpc_service ? rpc_service : "")
}' "<path_to_rpc_log_file>" > "<clean_path_to_rpc_log_file>"
```

### Validate:
You should have a list of permissions in the form of `service (rpc.method)` and `action (rpc.method)` you can then validate each permission needed using:

```bash
aws iam simulate-principal-policy \
    --policy-source-arn arn:aws:iam::000000000000:user/<user>@madetech.com \
    --action-names \
    sts:GetCallerIdentity ec2:DescribeVpcEndpointServices kms:CreateKey iam:CreateRole
```

### Turn off logging

Now for the clean up remember to run the following

```bash
unset TF_LOG
unset TF_LOG_PATH
```

## Further Investigtaion

### Generate Service Last Accessed Details

A simpler method may be togenerate a report to see what has been accessed, this can be used with the above as well. 

```
aws iam generate-service-last-accessed-details --arn arn:aws:iam::<account_id>:user/<user_name>
```

This will allow you to generate a report on what a user has access over a period of time (last 400 days). Providing you have a TF user in a sandbox that accesses nothing else, you will be able to see permissions neeed to run a partiuclar script.

[AWS CLI Command Reference - AWS Docs](https://docs.aws.amazon.com/cli/latest/reference/iam/generate-service-last-accessed-details.html)

### CloudTrail

Identify Necessary Permissions:

By analysing CloudTrail logs, you can determine exactly which actions your applications or users are performing in AWS. This data helps establish the specific permissions required, rather than granting broad access.
Aggregate CloudTrail Events:

Gather relevant CloudTrail events, particularly focusing on eventName fields, to capture the actions taken. This information highlights the services and operations accessed over time.
Generate Policies Based on Logs:

Use tools (such as AWS's Access Analyzer or third-party solutions) to convert these CloudTrail events into IAM policies. These tools compile a list of required actions, providing a customised policy that includes only the essential permissions.
Iterate for Precision:

Policies can initially be generated based on observed actions but should be refined over time. By monitoring ongoing CloudTrail logs, you can further adjust policies as usage patterns evolve.
Benefits of Least Privilege Policies:

Creating policies based on actual usage logs improves security by reducing the potential attack surface and ensuring users or applications have only the permissions they need.


[Least Priviledge - SymBlog](https://blog.symops.com/post/least-privilege-policies-from-aws-logs)
