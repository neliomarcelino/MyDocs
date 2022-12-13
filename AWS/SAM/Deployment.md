# Deployment

Best practice to shift traffic from the original Lambda function to the new Lambda function in the shortest time frame is with `Canary10Percent5Minutes`. With the Canary Deployment type, Traffic is shifted in two intervals. With Canary10Percent5Minutes, 10 percent of the traffic is shifted in the first interval while remaining all traffic is shifted after 5 minutes.

[More Info](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/automating-updates-to-serverless-apps.html)