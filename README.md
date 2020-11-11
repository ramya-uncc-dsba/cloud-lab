# cloud-top-review-lab
step 1: 
git clone https://github.com/rgummad1/cloud-top-review-lab to clone this repository

step 2:
Go into the repo directory. In this case: cd cloud-top-review-lab

step 3:
Make a "build" directory: mkdir build

step 4:
Compile the java code javac -cp /opt/cloudera/parcels/CDH/lib/hadoop/client/*:/opt/cloudera/parcels/CDH/lib/hbase/* top_review.java -d build -Xlint

step 5:
 wrap up our code into a Java "jar" file: jar -cvf reviews.jar -C build/ .
 
step 6:
Now we execute the map-reduce job: HADOOP_CLASSPATH=$(hbase mapredcp):/etc/hbase/conf hadoop jar reviews.jar top_review '/user/rgummad1/review_fields'
if needed remove the output that alreadt exists using : hadoop fs -rm -r /user/rgummad1/review_fields

step 7: 
concatenate the output across all output files with: hadoop fs -cat /user/rgummad1/review_fields/* 

step 8:
sorting to get top100: hadoop fs -cat /user/rgummad1/review_fields/* | sort -n -k2 -r | head -n100 
