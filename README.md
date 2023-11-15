
### AWS Amplify

#### Create a new Amplify Hosting project
```sh
aws amplify create-app --name <YourAppName> --repository <YourRepoUrl> --platform <Platform>
```
Replace `<YourAppName>` with your application name and `<YourRepoUrl>` with the URL of your repository. `<Platform>` can be `WEB`.

#### Connect your repository to Amplify
This is typically done through the AWS Management Console, but you can also set up a webhook using the AWS CLI:
```sh
aws amplify create-webhook --app-id <AppId> --branch-name <BranchName>
```
Replace `<AppId>` with the ID of your Amplify app and `<BranchName>` with the name of the branch you want to connect.

#### Modify your build output directory baseDirectory to /dist
You'll need to update the `amplify.yml` file in your repository to reflect the changes in the build output directory.

### S3 static website hosting

#### Create an S3 bucket with your project’s name
```sh
aws s3api create-bucket --bucket <BUCKET_NAME> --region <REGION> --create-bucket-configuration LocationConstraint=<REGION>
```
Replace `<BUCKET_NAME>` with your bucket name and `<REGION>` with the AWS region you want to create the bucket in.

#### Disable “Block all public access”
```sh
aws s3api put-public-access-block --bucket <BUCKET_NAME> --public-access-block-configuration "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
```

#### Upload your built files located in dist to S3
```sh
aws s3 cp dist/ s3://<BUCKET_NAME>/ --recursive
```

#### Update your bucket policy to allow public access
```sh
aws s3api put-bucket-policy --bucket <BUCKET_NAME> --policy file://policy.json
```
Create a `policy.json` file with the policy provided, replacing `<BUCKET_NAME>` with your bucket name.

#### Enable website hosting for your bucket
```sh
aws s3 website s3://<BUCKET_NAME>/ --index-document index.html --error-document 404.html
```

### S3 with CloudFront

#### Create an S3 bucket with your project’s name
(See the previous command for creating an S3 bucket.)

#### Upload your built files located in dist to S3
(See the previous command for uploading files to S3.)

#### Update your bucket policy to allow CloudFront Access
(See the previous command for updating the bucket policy, but use the policy provided for CloudFront access.)

#### CloudFront setup
To create a CloudFront distribution, you'll need to create a distribution configuration file (`distribution-config.json`) and then use the AWS CLI to create the distribution:
```sh
aws cloudfront create-distribution --distribution-config file://distribution-config.json
```
You'll need to fill out the `distribution-config.json` with the appropriate values as described.

#### CloudFront Functions setup
CloudFront Functions are not currently supported by the AWS CLI. You will need to use the AWS Management Console or AWS SDKs to create and manage CloudFront Functions.

Please note that the AWS CLI commands provided above are for guidance and may require additional parameters based on your specific setup and requirements. Always refer to the official AWS CLI documentation for the most accurate and up-to-date information.