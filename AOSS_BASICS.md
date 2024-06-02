# Amazon OpenSearch Service.

## Managed clusters vs Serverless vs ingestion

Amazon OpenSearch Service offers three main deployment options:

- Managed Clusters,
- Serverless,
- and Ingestion.

Each option is designed to cater to different use cases and requirements, providing flexibility in how you manage and scale your OpenSearch workloads.

### Managed Clusters

Managed Clusters are pre-provisioned OpenSearch clusters where you have control over the node types and capacity management. You are responsible for calculating storage requirements and choosing instance types for your domain. This option is suitable for scenarios where you need tight control over cluster configuration or specific customizations, such as data-sharding strategy [2][4].

Managed clusters require manual provisioning and management, including upgrades and service software updates. You pay for each hour of use of an EC2 instance and for the cumulative size of any EBS storage volumes attached to your instances [1].

### Serverless

Serverless is an on-demand, auto-scaling configuration that removes the operational complexities of provisioning, configuring, and tuning your OpenSearch clusters. It automatically scales compute capacity based on your application's needs, making it a cost-effective option for infrequent, intermittent, or unpredictable workloads [3][4].

Serverless collections are always encrypted, and you can choose the encryption key, but you can't disable encryption. It supports production workloads with redundancy for AZ outages and infrastructure failures, and you only pay for the resources consumed by your workload [3].

Serverless collections run OpenSearch version 2.0.x, and upgrades are managed automatically by AWS. It separates indexing (ingest) components from search (query) components, with Amazon S3 as the primary data storage for indexes, allowing for independent scaling of search and indexing functions [1].

### Ingestion

Ingestion refers to the process of collecting, filtering, enriching, transforming, normalizing, and aggregating data for downstream analysis and visualization. Amazon OpenSearch Ingestion is a fully managed data collector that delivers real-time log and trace data to Amazon OpenSearch Service domains and OpenSearch Serverless collections [4].

It is designed to work seamlessly with other AWS services, such as AWS KMS, IAM, AWS billing, CloudWatch, CloudFormation, and CloudTrail, providing a comprehensive solution for data ingestion and analysis [2].

## Choosing Between Managed Clusters, Serverless, and Ingestion

Managed Clusters are best for scenarios requiring tight control over cluster configuration or specific customizations, such as advanced features or plugins like alerting, anomaly detection, or kNN [2].

Serverless is ideal for organizations that don't want to self-manage their OpenSearch clusters or for those with infrequent, intermittent, or unpredictable workloads. It offers automatic provisioning, scaling, and encryption, making it simpler and more cost-effective [3][4].

Ingestion is crucial for efficiently collecting and preparing data for analysis, especially in scenarios involving real-time log and trace data. It complements both Managed Clusters and Serverless by providing a streamlined data ingestion process [4].

In summary, the choice between Managed Clusters, Serverless, and Ingestion depends on your specific needs, such as the level of control required, workload patterns, and the importance of cost-effectiveness and ease of management.

1. https://docs.aws.amazon.com/opensearch-service/latest/developerguide/serverless-overview.html
2. https://aws.amazon.com/opensearch-service/features/serverless/
3. https://docs.aws.amazon.com/opensearch-service/latest/developerguide/serverless.html
4. https://docs.aws.amazon.com/opensearch-service/latest/developerguide/what-is.html
5. https://medium.com/@blakeromano/aws-opensearch-serverless-vs-provisioned-b5385d6b20d8

## Knowledge base storage configuration provided is invalid...no such index

The error message you're encountering, "The knowledge base storage configuration provided is invalid... Request failed: [index_not_found_exception] no such index [bedrock-knowledge-base-default-index]," suggests that the Amazon Bedrock Knowledge Base AI is unable to locate the specified index in your OpenSearch cluster. This issue can arise due to several reasons, such as the index not being created, incorrect index name, or issues with the OpenSearch cluster itself.

To resolve this issue, you can follow these steps:

1. **Verify Index Existence**: First, ensure that the index `bedrock-knowledge-base-default-index` exists in your OpenSearch cluster. You can do this by listing all indices in your cluster. If the index does not exist, you will need to create it.

2. **Create the Index**: If the index does not exist, you can create it manually through the OpenSearch dashboard or programmatically using a Python library or any other method supported by OpenSearch. For example, you can use the Python library to create the index as suggested in the expert's response [1].

3. **Check OpenSearch Cluster Health**: Ensure that your OpenSearch cluster is healthy and operational. Sometimes, issues with the cluster itself can lead to failures in fetching or creating indices.

4. **Review Storage Configuration**: According to the AWS documentation, the storage configuration for the knowledge base in Amazon OpenSearch Service should be correctly set up. Ensure that the storage configuration in your Bedrock setup matches the expected format and includes all necessary details [2].

5. **Permissions and Policies**: Verify that the IAM roles and policies associated with your OpenSearch cluster and Bedrock setup have the necessary permissions to create and access indices. Incorrect permissions can lead to failures in index operations.

6. **Consult AWS Documentation and Support**: If the issue persists, consult the AWS documentation for Bedrock and OpenSearch Service for any additional configuration steps or known issues. AWS support can also provide guidance specific to your account and setup.

Here's a basic example of how you might create an index using the OpenSearch REST API, which you can adapt to your specific needs:

```bash
curl -X PUT "localhost:9200/bedrock-knowledge-base-default-index" -H 'Content-Type: application/json' -d'
{
 "settings" : {
    "index" : {
      "number_of_shards" : 1,
      "number_of_replicas" : 1
    }
 }
}'
```

Replace `localhost:9200` with your OpenSearch cluster's endpoint and adjust the settings as necessary for your use case.

Remember, the exact steps and commands might vary based on your specific setup and requirements.

Citations:
[1] https://repost.aws/questions/QUFeMBrx3BSDaQ08ipkXBePw/amzon-bedrock-error
[2] https://docs.aws.amazon.com/bedrock/latest/APIReference/API_agent_StorageConfiguration.html
[3] https://repost.aws/es/questions/QUFeMBrx3BSDaQ08ipkXBePw/amzon-bedrock-error
[4] https://github.com/aws-samples/bedrock-kb-rag-workshop/issues/1
[5] https://stackoverflow.com/questions/41804863/error-index-not-found-exception
[6] https://community.graylog.org/t/graylog-5-with-opensearch-unable-to-start-because-elasticsearch-is-not-installed/26847
[7] https://docs.aws.amazon.com/opensearch-service/latest/developerguide/handling-errors.html
[8] https://community.splunk.com/t5/Deployment-Architecture/Error-Message-on-indexer-console/m-p/464714
[9] https://confluence.atlassian.com/display/CONFKB/Searching+and+Indexing+Troubleshooting
[10] https://github.com/expo/expo/issues/17511
