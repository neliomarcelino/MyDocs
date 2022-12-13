# Invocation

Writing a code that avoids the recursive call of the function within itself as it is not recommended as a best practice.
 
For the "Lambda function code’ best practice, it is recommended that we should avoid recursive code in the Lambda function. 

> "Avoid using recursive code in your Lambda function, wherein the function automatically calls itself until some arbitrary criteria are met. This could lead to an unintended volume of function invocations and escalated costs. If you do accidentally do so, set the function concurrent execution limit to ‘0' (Zero) immediately to throttle all invocations to the function, while you update the code.”

