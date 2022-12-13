> Use the AWS CLI command to specify different parameters that need to be run for the build. Since the developer can run the build, he can run the build by changing the parameters from the command line. The same is also mentioned in the AWS Documentation.
> ![[Pasted image 20221202222630.png]]
> [More info](https://docs.aws.amazon.com/codebuild/latest/userguide/run-build.html#run-build-cli)

> [!Note]
> If the developer doesn't have access to edit the code build project but only have access to run the build, he can make use of the `BuildspecOverride` attribute.
> 
> As per  AWS:
> ```
> buildspecOverride
>:Optional string. A build spec declaration that overrides for this build one defined in the build project. If this value is set, it can be either an inline build spec definition or the path to an alternate build spec file relative to the values of the built-in
