### Run a Python application on Hadoop Yarn on Big Data Platform
```bash
#!/bin/bash
# Assign the application name and do submit the job if it is not currently running
# Set spark3 environment, otherwise the application submitting by spark3-submit will not launch via crontab 
# Example for spark3-submit
source /etc/spark3/bin/spark3-env
app_name=SparkPi
app_is_running=$(yarn application -list|grep $app_name)
 
if [[ $app_is_running == *"$app_name"* ]]
then
    echo "Your streaming application is running. Please stop it before resubmit the Spark job."
else
    /etc/spark3/bin/spark3-submit --master yarn \
    --name $app_name \
    --deploy-mode cluster \
    --driver-memory 1g \
    --executor-memory 1g \
    --executor-cores 1 \
    --conf spark.dynamicAllocation.enabled=false \
    /opt/cloudera/parcels/SPARK3_3/lib/python3.9/site-packages/pyspark/examples/src/main/python/pi.py
    echo "$app_name is just submitted to Yarn. Please check the status on Yarn UI."
fi
```