아래는 로컬 PC 의 주피터 노트북을 이용해서 AWS Glue 와 interactive session 를 설정하는 데 필요한 과정을 나열한 것입니다..

* 패키지 설치
```
% pip3 install --upgrade jupyter boto3 aws-glue-sessions

% SITE_PACKAGES=$(pip3 show aws-glue-sessions | grep Location | awk '{print $2}')
% sudo jupyter kernelspec install $SITE_PACKAGES/aws_glue_interactive_sessions_kernel/glue_pyspark
% sudo jupyter kernelspec install $SITE_PACKAGES/aws_glue_interactive_sessions_kernel/glue_spark 

% jupyter kernelspec list
Available kernels:
  python3               /opt/anaconda3/share/jupyter/kernels/python3
  apache_toree_scala    /usr/local/share/jupyter/kernels/apache_toree_scala
  glue_pyspark          /usr/local/share/jupyter/kernels/glue_pyspark
  glue_spark            /usr/local/share/jupyter/kernels/glue_spark
  spylon-kernel         /usr/local/share/jupyter/kernels/spylon-kernel
```

* 유저 권한 설정

```
% aws sts get-caller-identity
{
    "UserId": "AIDAXITLFFBW6ZYSNWLEZ",
    "Account": "xxxxxxxxx",
    "Arn": "arn:aws:iam::xxxxxxxxx:user/username"
}

% aws iam attach-user-policy --user-name username --policy-arn arn:aws:iam::aws:policy/AWSGlueConsoleFullAccess

```



* 주피터 노트북 
```
 % jupyter notebook
[I 21:50:22.545 NotebookApp] Loading IPython parallel extension
[I 2023-04-03 21:50:25.435 LabApp] JupyterLab extension loaded from /opt/anaconda3/lib/python3.9/site-packages/jupyterlab
[I 2023-04-03 21:50:25.435 LabApp] JupyterLab application directory is /opt/anaconda3/share/jupyter/lab
[I 21:50:25.439 NotebookApp] Serving notebooks from local directory: /Users/soonbeom
[I 21:50:25.439 NotebookApp] Jupyter Notebook 6.4.12 is running at:
[I 21:50:25.439 NotebookApp] http://localhost:8888/?token=ee2b6a66b831feafe9f28132b4db7373cceb3625bdea4c3d
[I 21:50:25.439 NotebookApp]  or http://127.0.0.1:8888/?token=ee2b6a66b831feafe9f28132b4db7373cceb3625bdea4c3d
[I 21:50:25.439 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 21:50:25.442 NotebookApp]

    To access the notebook, open this file in a browser:
        file:///Users/soonbeom/Library/Jupyter/runtime/nbserver-24374-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/?token=ee2b6a66b831feafe9f28132b4db7373cceb3625bdea4c3d
     or http://127.0.0.1:8888/?token=ee2b6a66b831feafe9f28132b4db7373cceb3625bdea4c3d
```



## 참고자료 ##

* https://aws.amazon.com/blogs/big-data/introducing-aws-glue-interactive-sessions-for-jupyter/

* AWS Tutorials - Interactively Develop Glue Job using Jupyter Notebook in GlueStudio

  https://www.youtube.com/watch?v=n4PVC5O_tJo
  
  
* AWS Glue Tutorial for Beginners [FULL COURSE in 45 mins]

  https://www.youtube.com/watch?v=dQnRP6X8QAU
