# Creating and managing Amazon S3 on Outposts buckets<a name="S3OutpostsBucketJava"></a>

You can use the SDK for Java to create and manage your S3 on Outposts buckets\. The following examples show how to work with Outposts buckets, access points, lifecycle configurations, bucket policies, access point policies, and endpoints\.

**Topics**
+ [Configure S3 control client for S3 on Outposts](#S3OutpostsCongfigureS3ControlClientJava)
+ [Create an S3 on Outposts bucket](#S3OutpostsCreateBucketJava)
+ [Get an S3 on Outposts bucket](#S3OutpostsGetBucketJava)
+ [Get a list of buckets in an Outpost](#S3OutpostsListRegionalBucketJava)
+ [Create an access point for an S3 on Outposts bucket](#S3OutpostsCreateAccessPointJava)
+ [Get an access point for an S3 on Outposts bucket](#S3OutpostsGetAccessPointJava)
+ [List access points for an S3 on Outposts bucket](#S3OutpostsListAccessPointJava)
+ [Put a lifecycle configuration on an S3 on Outposts bucket](#S3OutpostsPutBucketLifecycleConfigurationJava)
+ [Get a lifecycle configuration for an S3 on Outposts bucket](#S3OutpostsGetBucketLifecycleConfigurationJava)
+ [Put a policy on your S3 on Outposts bucket](#S3OutpostsPutBucketPolicyJava)
+ [Get a policy for an S3 on Outposts bucket](#S3OutpostsGetBucketPolicyJava)
+ [Put a policy on your S3 on Outposts access point](#S3OutpostsPutAccessPointPolicyJava)
+ [Get a policy for an S3 on Outposts access point](#S3OutpostsGetAccessPointPolicyJava)
+ [Create an endpoint for an Outpost](#S3OutpostsCreateEndpointJava)
+ [Delete an endpoint for an Outpost](#S3OutpostsDeleteEndpointJava)
+ [List endpoints for S3 on Outposts](#S3OutpostsListEndpointsJava)

## Configure S3 control client for S3 on Outposts<a name="S3OutpostsCongfigureS3ControlClientJava"></a>

The following example configures the S3 control client for S3 on Outposts using the SDK for Java\. 

```
import com.amazonaws.auth.AWSStaticCredentialsProvider;
import com.amazonaws.auth.BasicAWSCredentials;
import com.amazonaws.services.s3control.AWSS3Control;
import com.amazonaws.services.s3control.AWSS3ControlClient;

public AWSS3Control createS3ControlClient() {

    String accessKey = AWSAccessKey;
    String secretKey = SecretAccessKey;
    BasicAWSCredentials awsCreds = new BasicAWSCredentials(accessKey, secretKey);

    return AWSS3ControlClient.builder().enableUseArnRegion()
            .withCredentials(new AWSStaticCredentialsProvider(awsCreds))
            .build();

}
```

## Create an S3 on Outposts bucket<a name="S3OutpostsCreateBucketJava"></a>

The following example creates an S3 on Outposts bucket \(`s3-outposts:CreateBucket`\) using the SDK for Java\. 

```
import com.amazonaws.services.s3control.model.*;

public String createBucket(String bucketName) {

    CreateBucketRequest reqCreateBucket = new CreateBucketRequest()
            .withBucket(bucketName)
            .withOutpostId(OutpostId)
            .withCreateBucketConfiguration(new CreateBucketConfiguration());

    CreateBucketResult respCreateBucket = s3ControlClient.createBucket(reqCreateBucket);
    System.out.printf("CreateBucket Response: %s%n", respCreateBucket.toString());

    return respCreateBucket.getBucketArn();

}
```

## Get an S3 on Outposts bucket<a name="S3OutpostsGetBucketJava"></a>

The following S3 on Outposts example gets a bucket using the SDK for Java\. 

```
import com.amazonaws.services.s3control.model.*;

public void getBucket(String bucketArn) {

    GetBucketRequest reqGetBucket = new GetBucketRequest()
            .withBucket(bucketArn)
            .withAccountId(AccountId);

    GetBucketResult respGetBucket = s3ControlClient.getBucket(reqGetBucket);
    System.out.printf("GetBucket Response: %s%n", respGetBucket.toString());

}
```

## Get a list of buckets in an Outpost<a name="S3OutpostsListRegionalBucketJava"></a>

The following SDK for Java example gets a list of buckets in an Outpost\. 

```
import com.amazonaws.services.s3control.model.*;

public void listRegionalBuckets() {

    ListRegionalBucketsRequest reqListBuckets = new ListRegionalBucketsRequest()
            .withAccountId(AccountId)
            .withOutpostId(OutpostId);

    ListRegionalBucketsResult respListBuckets = s3ControlClient.listRegionalBuckets(reqListBuckets);
    System.out.printf("ListRegionalBuckets Response: %s%n", respListBuckets.toString());

}
```

## Create an access point for an S3 on Outposts bucket<a name="S3OutpostsCreateAccessPointJava"></a>

The following SDK for Java example creates an access point for an Outposts bucket\.

```
import com.amazonaws.services.s3control.model.*;

public String createAccessPoint(String bucketArn, String accessPointName) {

    CreateAccessPointRequest reqCreateAP = new CreateAccessPointRequest()
            .withAccountId(AccountId)
            .withBucket(bucketArn)
            .withName(accessPointName)
            .withVpcConfiguration(new VpcConfiguration().withVpcId("vpc-12345"));

    CreateAccessPointResult respCreateAP = s3ControlClient.createAccessPoint(reqCreateAP);
    System.out.printf("CreateAccessPoint Response: %s%n", respCreateAP.toString());

    return respCreateAP.getAccessPointArn();

}
```

## Get an access point for an S3 on Outposts bucket<a name="S3OutpostsGetAccessPointJava"></a>

The following SDK for Java example gets an access point for an Outposts bucket\.

```
import com.amazonaws.services.s3control.model.*;

public void getAccessPoint(String accessPointArn) {

    GetAccessPointRequest reqGetAP = new GetAccessPointRequest()
            .withAccountId(AccountId)
            .withName(accessPointArn);

    GetAccessPointResult respGetAP = s3ControlClient.getAccessPoint(reqGetAP);
    System.out.printf("GetAccessPoint Response: %s%n", respGetAP.toString());

}
```

## List access points for an S3 on Outposts bucket<a name="S3OutpostsListAccessPointJava"></a>

The following SDK for Java example lists the access points for an Outposts bucket\.

```
import com.amazonaws.services.s3control.model.*;

public void listAccessPoints(String bucketArn) {

    ListAccessPointsRequest reqListAPs = new ListAccessPointsRequest()
            .withAccountId(AccountId)
            .withBucket(bucketArn);

    ListAccessPointsResult respListAPs = s3ControlClient.listAccessPoints(reqListAPs);
    System.out.printf("ListAccessPoints Response: %s%n", respListAPs.toString());

}
```

## Put a lifecycle configuration on an S3 on Outposts bucket<a name="S3OutpostsPutBucketLifecycleConfigurationJava"></a>

The following SDK for Java example puts a lifecycle configuration on an Outposts bucket\. This lifecycle configuration specifies that all objects that have the flagged prefix \(`myprefix`\) and tags expire after 10 days\.

```
import com.amazonaws.services.s3control.model.*;

public void putBucketLifecycleConfiguration(String bucketArn) {

    S3Tag tag1 = new S3Tag().withKey("mytagkey1").withValue("mytagvalue1");
    S3Tag tag2 = new S3Tag().withKey("mytagkey2").withValue("mytagvalue2");

    LifecycleRuleFilter lifecycleRuleFilter = new LifecycleRuleFilter()
            .withAnd(new LifecycleRuleAndOperator()
                    .withPrefix("myprefix")
                    .withTags(tag1, tag2));

    LifecycleExpiration lifecycleExpiration = new LifecycleExpiration()
            .withExpiredObjectDeleteMarker(false)
            .withDays(10);

    LifecycleRule lifecycleRule = new LifecycleRule()
            .withStatus("Enabled")
            .withFilter(lifecycleRuleFilter)
            .withExpiration(lifecycleExpiration)
            .withID("id-1");


    LifecycleConfiguration lifecycleConfiguration = new LifecycleConfiguration()
            .withRules(lifecycleRule);

    PutBucketLifecycleConfigurationRequest reqPutBucketLifecycle = new PutBucketLifecycleConfigurationRequest()
            .withAccountId(AccountId)
            .withBucket(bucketArn)
            .withLifecycleConfiguration(lifecycleConfiguration);

    PutBucketLifecycleConfigurationResult respPutBucketLifecycle = s3ControlClient.putBucketLifecycleConfiguration(reqPutBucketLifecycle);
    System.out.printf("PutBucketLifecycleConfiguration Response: %s%n", respPutBucketLifecycle.toString());


}
```

## Get a lifecycle configuration for an S3 on Outposts bucket<a name="S3OutpostsGetBucketLifecycleConfigurationJava"></a>

The following SDK for Java example gets a lifecycle configuration for an Outposts bucket\.

```
import com.amazonaws.services.s3control.model.*;

public void getBucketLifecycleConfiguration(String bucketArn) {

    GetBucketLifecycleConfigurationRequest reqGetBucketLifecycle = new GetBucketLifecycleConfigurationRequest()
            .withAccountId(AccountId)
            .withBucket(bucketArn);

    GetBucketLifecycleConfigurationResult respGetBucketLifecycle = s3ControlClient.getBucketLifecycleConfiguration(reqGetBucketLifecycle);
    System.out.printf("GetBucketLifecycleConfiguration Response: %s%n", respGetBucketLifecycle.toString());

}
```

## Put a policy on your S3 on Outposts bucket<a name="S3OutpostsPutBucketPolicyJava"></a>

The following SDK for Java example puts a policy on an Outposts bucket\.

```
import com.amazonaws.services.s3control.model.*;

public void putBucketPolicy(String bucketArn) {

    String policy = "{\"Version\":\"2012-10-17\",\"Id\":\"testBucketPolicy\",\"Statement\":[{\"Sid\":\"st1\",\"Effect\":\"Allow\",\"Principal\":{\"AWS\":\"" + AccountId+ "\"},\"Action\":\"s3-outposts:*\",\"Resource\":\"" + bucketArn + "\"}]}";

    PutBucketPolicyRequest reqPutBucketPolicy = new PutBucketPolicyRequest()
            .withAccountId(AccountId)
            .withBucket(bucketArn)
            .withPolicy(policy);

    PutBucketPolicyResult respPutBucketPolicy = s3ControlClient.putBucketPolicy(reqPutBucketPolicy);
    System.out.printf("PutBucketPolicy Response: %s%n", respPutBucketPolicy.toString());

}
```

## Get a policy for an S3 on Outposts bucket<a name="S3OutpostsGetBucketPolicyJava"></a>

The following SDK for Java example gets a policy for an Outposts bucket\.

```
import com.amazonaws.services.s3control.model.*;

public void getBucketPolicy(String bucketArn) {

    GetBucketPolicyRequest reqGetBucketPolicy = new GetBucketPolicyRequest()
            .withAccountId(AccountId)
            .withBucket(bucketArn);

    GetBucketPolicyResult respGetBucketPolicy = s3ControlClient.getBucketPolicy(reqGetBucketPolicy);
    System.out.printf("GetBucketPolicy Response: %s%n", respGetBucketPolicy.toString());

}
```

## Put a policy on your S3 on Outposts access point<a name="S3OutpostsPutAccessPointPolicyJava"></a>

The following SDK for Java example puts a policy on an Outposts access point\.

```
import com.amazonaws.services.s3control.model.*;

public void putAccessPointPolicy(String accessPointArn) {

    String policy = "{\"Version\":\"2012-10-17\",\"Id\":\"testAccessPointPolicy\",\"Statement\":[{\"Sid\":\"st1\",\"Effect\":\"Allow\",\"Principal\":{\"AWS\":\"" + AccountId + "\"},\"Action\":\"s3-outposts:*\",\"Resource\":\"" + accessPointArn + "\"}]}";

    PutAccessPointPolicyRequest reqPutAccessPointPolicy = new PutAccessPointPolicyRequest()
            .withAccountId(AccountId)
            .withName(accessPointArn)
            .withPolicy(policy);

    PutAccessPointPolicyResult respPutAccessPointPolicy = s3ControlClient.putAccessPointPolicy(reqPutAccessPointPolicy);
    System.out.printf("PutAccessPointPolicy Response: %s%n", respPutAccessPointPolicy.toString());
    printWriter.printf("PutAccessPointPolicy Response: %s%n", respPutAccessPointPolicy.toString());

}
```

## Get a policy for an S3 on Outposts access point<a name="S3OutpostsGetAccessPointPolicyJava"></a>

The following SDK for Java example gets a policy for an Outposts access point\.

```
import com.amazonaws.services.s3control.model.*;

public void getAccessPointPolicy(String accessPointArn) {

    GetAccessPointPolicyRequest reqGetAccessPointPolicy = new GetAccessPointPolicyRequest()
            .withAccountId(AccountId)
            .withName(accessPointArn);

    GetAccessPointPolicyResult respGetAccessPointPolicy = s3ControlClient.getAccessPointPolicy(reqGetAccessPointPolicy);
    System.out.printf("GetAccessPointPolicy Response: %s%n", respGetAccessPointPolicy.toString());
    printWriter.printf("GetAccessPointPolicy Response: %s%n", respGetAccessPointPolicy.toString());

}
```

## Create an endpoint for an Outpost<a name="S3OutpostsCreateEndpointJava"></a>

The following SDK for Java example creates an endpoint for an Outpost\.

```
import com.amazonaws.services.s3outposts.AmazonS3Outposts;
import com.amazonaws.services.s3outposts.AmazonS3OutpostsClientBuilder;
import com.amazonaws.services.s3outposts.model.CreateEndpointRequest;
import com.amazonaws.services.s3outposts.model.CreateEndpointResult;

public void createEndpoint() {
    AmazonS3Outposts s3OutpostsClient = AmazonS3OutpostsClientBuilder
                .standard().build();
                
    CreateEndpointRequest createEndpointRequest = new CreateEndpointRequest()
                .withOutpostId("op-0d79779cef3c30a40")
                .withSubnetId("subnet-8c7a57c5")
                .withSecurityGroupId("sg-ab19e0d1")		
                .withAccessType("CustomerOwnedIp")   
                .withCustomerOwnedIpv4Pool("ipv4pool-coip-12345678901234567");
    // Use .withAccessType and .withCustomerOwnedIpv4Pool only when the access type is 
    // customer-owned IP address pool (CoIP pool) 
    CreateEndpointResult createEndpointResult = s3OutpostsClient.createEndpoint(createEndpointRequest);
    System.out.println("Endpoint is created and its ARN is " + createEndpointResult.getEndpointArn());
}
```

## Delete an endpoint for an Outpost<a name="S3OutpostsDeleteEndpointJava"></a>

The following SDK for Java example deletes an endpoint for an Outpost\.

```
import com.amazonaws.arn.Arn;
import com.amazonaws.services.s3outposts.AmazonS3Outposts;
import com.amazonaws.services.s3outposts.AmazonS3OutpostsClientBuilder;
import com.amazonaws.services.s3outposts.model.DeleteEndpointRequest;

public void deleteEndpoint(String endpointArnInput) {
    String outpostId = "op-01ac5d28a6a232904";
    AmazonS3Outposts s3OutpostsClient = AmazonS3OutpostsClientBuilder
                .standard().build();
                
    Arn endpointArn = Arn.fromString(endpointArnInput);
    String[] resourceParts = endpointArn.getResource().getResource().split("/");
    String endpointId = resourceParts[resourceParts.length - 1];
    DeleteEndpointRequest deleteEndpointRequest = new DeleteEndpointRequest()
                .withEndpointId(endpointId)
                .withOutpostId(outpostId);
    s3OutpostsClient.deleteEndpoint(deleteEndpointRequest);
    System.out.println("Endpoint with id " + endpointId + " is deleted.");
}
```

## List endpoints for S3 on Outposts<a name="S3OutpostsListEndpointsJava"></a>

The following SDK for Java example lists the endpoints for an Outpost\.

```
import com.amazonaws.services.s3outposts.AmazonS3Outposts;
import com.amazonaws.services.s3outposts.AmazonS3OutpostsClientBuilder;
import com.amazonaws.services.s3outposts.model.ListEndpointsRequest;
import com.amazonaws.services.s3outposts.model.ListEndpointsResult;

public void listEndpoints() {
    AmazonS3Outposts s3OutpostsClient = AmazonS3OutpostsClientBuilder
                .standard().build();
                
    ListEndpointsRequest listEndpointsRequest = new ListEndpointsRequest();
    ListEndpointsResult listEndpointsResult = s3OutpostsClient.listEndpoints(listEndpointsRequest);
    System.out.println("List endpoints result is " + listEndpointsResult);
}
```