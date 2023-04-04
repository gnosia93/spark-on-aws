아래는 로컬 PC 의 주피터 노트북을 이용해서 AWS Glue 와 interactive session 를 설정하는 데 필요한 과정을 나열한 것입니다..

### 1. 패키지 설치 ###
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
커널 리스트에 glue_pyspark 과 glue_spark 이 보이면 정상적으로 설치된 것이다. 

### 2. 유저 권한 설정 ###

```
% aws sts get-caller-identity
{
    "UserId": "AIDAXITLFFBW6ZYSNWLEZ",
    "Account": "xxxxxxxxx",
    "Arn": "arn:aws:iam::xxxxxxxxx:user/soonbeom"
}

% aws iam attach-user-policy --user-name soonbeom --policy-arn arn:aws:iam::aws:policy/AWSGlueConsoleFullAccess
```

### 3. Glue 서비스롤 생성 ###

IAM 에서 AWSGlueServiceRoleJupyter 라는 서비스 롤을 생성한다. 롤의 Trusted Entity 는 Glue 이고, attach 되는 권한은 매니지드 권한인 AmazonS3FullAccess 와 AWSGlueServiceRole 이다. 생성된 롤은 주피터 노트북에서 인터렉티브 한 모드로 glue 와 통신하기 위해 사용되는데,   
주피터 노트북에서는 %iam_role arn:aws:iam::0000000000:role/AWSGlueServiceRoleJupyter 과 매직을 사용하게 된다.   



### 4. 주피터 노트북 실행 ### 
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
[New] -> [Glue Pyspark] 을 선택해서 새로운 노트북을 하나 띄운다. 


### 5. 샘플 프로그램 작성 ###

샘플 프로그램을 작성하기 전에 클라우드 포메이션으로 데이터를 먼저 빌드한다. us-east-2 리전의 glue 서비스에 COVID-19 데이터베이스와 관련 테이블 들이 생성도고, 원본 S3 로 경로가 설정된다.(cross account)  
https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/quickcreate?templateURL=https://covid19-lake.s3.us-east-2.amazonaws.com/cfn/CovidLakeStack.template.json&stackName=CovidLakeStack



https://github.com/gnosia93/spark-on-aws/blob/main/glue/covid-19.ipynb

![](https://github.com/gnosia93/spark-on-aws/blob/main/glue/covid-19.png)

### 6. 파일변환 및 S3 업로드 ###

```
%glue_version 2.0
%number_of_workers 2
%worker_type G.2X
%idle_timeout 60
%region us-east-2
%iam_role arn:aws:iam::0000000000:role/AWSGlueServiceRoleJupyter 
```
```
%%sql 
show tables in `covid-19`
```

등과 같은 ipython 매직은 nbconvert 를 이용하더라도 소스 코드에서 지워지지 않은 관계로, 변환후 S3로 업로드 전에 수동으로 지워야 한다. 

```
% jupyter nbconvert --to script covid-19.ipynb

% aws s3 cp covid-19.py s3://glue-sink-20230404
```



### 7. glue 잡 등록 ###
```
% aws glue create-job \
--name covid-demo \
--role "AWSGlueServiceRoleJupyter" \
--command '{"Name": "covid-demo-etl", "PythonVersion": "3", "ScriptLocation": "s3://glue-sink-20230404/covid-19.py"}' \
--default-arguments '{"--enable-glue-datacatalog": "true"}' \
--number-of-workers 2 \
--worker-type G.2X \
--region us-east-2
{
    "Name": "covid-demo"
}
```

## 참고자료 ##

* https://aws.amazon.com/blogs/big-data/introducing-aws-glue-interactive-sessions-for-jupyter/

* AWS Tutorials - Interactively Develop Glue Job using Jupyter Notebook in GlueStudio

  https://www.youtube.com/watch?v=n4PVC5O_tJo
  
  
* AWS Glue Tutorial for Beginners [FULL COURSE in 45 mins]

  https://www.youtube.com/watch?v=dQnRP6X8QAU
