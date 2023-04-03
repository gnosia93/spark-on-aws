* 패키지 설치
```
% pip3 install --upgrade jupyter boto3 aws-glue-sessions

% SITE_PACKAGES=$(pip3 show aws-glue-sessions | grep Location | awk '{print $2}')
% sudo jupyter kernelspec install $SITE_PACKAGES/aws_glue_interactive_sessions_kernel/glue_pyspark
% sudo jupyter kernelspec install $SITE_PACKAGES/aws_glue_interactive_sessions_kernel/glue_spark 
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

* AWS Tutorials - Interactively Develop Glue Job using Jupyter Notebook

 https://www.youtube.com/watch?v=n4PVC5O_tJo
