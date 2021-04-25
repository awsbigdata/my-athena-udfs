# my-athena-udfs


1. Clone the federation SDK 

git clone https://github.com/awslabs/aws-athena-query-federation.git

2. go to aws-athena-query-federation folder 
 
cd aws-athena-query-federation

3. Run below command on new env 

bash tools/prepare_dev_env.sh

4. Add to source file 
    source ~/.profile

5. Build SDK Jar using mvn command 

mvn clean install -DskipTests

6. Clone UDF git repo 

git clone https://github.com/awsbigdata/my-athena-udfs.git

7. go to udf repo folder and edit pom.xml aws-athena-federation-sdk version as per step 1 output

8. Run mvn command to build a JAR 

mvn clean install -DskipTests

9. Create a lambda function (make sure that lambda role is created before this)

aws lambda create-function \
 --function-name athenaudf \
 --runtime java8 \
 --role arn:aws:iam::3242345446345:role/serverlessrepo-AthenaUserDefin \
 --handler com.awssupport.athena.udfs.MyUserDefinedFunctions \
 --timeout 900 \
 --zip-file fileb://./target/my-athena-udfs-1.0-SNAPSHOT.jar --region us-west-2

10. Go to Athena and make sure that Console is pointing to Engine version 2

11. Run below query to test UDF 

USING EXTERNAL FUNCTION compress(input VARCHAR)
RETURNS VARCHAR
LAMBDA 'athenaudf'
SELECT  compress('Hello')
