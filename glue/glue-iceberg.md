### 1. sample 데이터베이스 생성 ###

glue 콘솔에서 sample 데이터베이스를 생성한다. 

### 2. flight 테이블 생성하기 ###

  glue 서비스에서 Data Catalog 하위에 있는 Tables 메뉴로 이동하여 [Add tables using crawler] 를 선택한 후,  
  s3://crawler-public-us-west-2/flight/2016/csv/ 경로에 대해서 tbl-csv 테이블을 sample 데이터베이스 밑에 생성한다. 
  
  ![](https://github.com/gnosia93/spark-on-aws/blob/main/glue/image/glue-table.png)



### 오류 메시지 ###

* An error occurred while calling o92.sql. Cannot find catalog plugin class for catalog 'glue_catalog': org.apache.iceberg.spark.SparkCatalog

--> job 파리미터를 설정한다.

--datalake-formats = iceberg



## 참고자료 ##

* [AWS Glue Data Catalog 시작하기](https://docs.aws.amazon.com/ko_kr/glue/latest/dg/start-data-catalog.html)

* [AWS Tutorials - Creating Glue Job with Apache Iceberg Table](https://www.youtube.com/watch?v=wDpg0wiAyE0)
