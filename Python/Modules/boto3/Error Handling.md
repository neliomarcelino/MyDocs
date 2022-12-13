> Exceptions that you might encounter when using Boto3 will come from one of two sources: botocore or the AWS services your client is interacting with.


## Botocore Exceptions
---
> These exceptions are statically defined within the botocore package, a dependency of Boto3. The exceptions are related to issues with client-side behaviors, configurations, or validations. You can generate a list of the statically defined botocore exceptions using the following code:

```python
import botocore.exceptions

for key, value in sorted(botocore.exceptions.__dict__.items()):
    if isinstance(value, type):
        print(key)
```


## Catching botocore exceptions
---
> Botocore exceptions are statically defined in the botocore package. Any Boto3 clients you create will use these same statically defined exception classes. The most common botocore exception you’ll encounter is `ClientError`. This is a general exception when an error response is provided by an AWS service to your Boto3 client’s request.

>Additional client-side issues with SSL negotiation, client misconfiguration, or AWS service validation errors will also throw botocore exceptions. Here’s a generic example of how you might catch botocore exceptions.

```python
import botocore
import boto3

client = boto3.client('aws_service_name')

try:
    client.some_api_call(SomeParam='some_param')

except botocore.exceptions.ClientError as error:
    # Put your error handling logic here
    raise error

except botocore.exceptions.ParamValidationError as error:
    raise ValueError('The parameters you provided are incorrect: {}'.format(error))
```

> Boto3 Exception dict example:
```json
{
    'Error': {
        'Code': 'SomeServiceException',
        'Message': 'Details/context around the exception or error'
    },
    'ResponseMetadata': {
        'RequestId': '1234567890ABCDEF',
        'HostId': 'host ID data will appear here as a hash',
        'HTTPStatusCode': 400,
        'HTTPHeaders': {'header metadata key/values will appear here'},
        'RetryAttempts': 0
    }
}
```

> Catching a specific Error Code:
```python
import botocore
import boto3
import logging

# Set up our logger
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger()

client = boto3.client('kinesis')

try:
    logger.info('Calling DescribeStream API on myDataStream')
    client.describe_stream(StreamName='myDataStream')

except botocore.exceptions.ClientError as error:
    if error.response['Error']['Code'] == 'LimitExceededException':
        logger.warn('API call limit exceeded; backing off and retrying...')
    else:
        raise error
```

>[!Note]
>The Boto3 `standard` retry mode will catch throttling errors and exceptions, and will back off and retry them for you.
>

---
Tags:
#aws/boto3/error_handling
Links:
- Official Documentation: https://boto3.amazonaws.com/v1/documentation/api/latest/guide/error-handling.html