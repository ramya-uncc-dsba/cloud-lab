# cloud-top_review_lab
Log into dsba-hadoop.uncc.edu using ssh
git clone https://github.com/rgummad1/cloud-top-review-lab to clone this repo
Go into the repo directory. In this case: cd cloud-top-review-lab
Make a "build" directory (if it does not already exist): mkdir build
Compile the java code (all one line). You may see some warnings--that' ok. javac -cp /opt/cloudera/parcels/CDH/lib/hadoop/client/*:/opt/cloudera/parcels/CDH/lib/hbase/* top-review.java -d build -Xlint
Now we wrap up our code into a Java "jar" file: jar -cvf reviews.jar -C build/ .
This is the final step
Note that you will need to delete the output folder if it already exists: hadoop fs -rm -r /user/rgummad1/review_fields otherwise you will get an "Exception in thread "main" org.apache.hadoop.mapred.FileAlreadyExistsException: Output directory hdfs://dsba-nameservice/user/... type of error.
Now we execute the map-reduce job: HADOOP_CLASSPATH=$(hbase mapredcp):/etc/hbase/conf hadoop jar reviews.jar top-review '/user/rgummad1/review_fields'
Once that job completes, you can concatenate the output across all output files with: hadoop fs -cat /user/rgummad1/review_fields/* | sort -n -k2 -r | head -n100 or if you have output that is too big for displaying on the terminal screen you can do hadoop fs -cat /user/rfox12/product_fields/* | sort -n -k2 -r | head -n100 > output.txt to redirect all output to output.txt
