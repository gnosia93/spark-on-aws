
```
% pip3 install --upgrade jupyter boto3 aws-glue-sessions

% SITE_PACKAGES=$(pip3 show aws-glue-sessions | grep Location | awk '{print $2}')
% sudo jupyter kernelspec install $SITE_PACKAGES/aws_glue_interactive_sessions_kernel/glue_pyspark
% sudo jupyter kernelspec install $SITE_PACKAGES/aws_glue_interactive_sessions_kernel/glue_spark 
```
