## Word Count (3pt)
Write a Spark script that outputs the count number of each word. In the implementation, you need to transform all words as lower case and filter out all the punctuations.
Run your program over the input data ‘pg100.txt’

What to submit: Submit the printout of the output file and the source code.


```python

```

# 1.Install

os: ubuntu

python: 2.7

spark: 2.3

```
# get image
docker pull bde2020/spark-master

# start a container in backstage and mapping local file to container
docker run --name spark-master -h spark-master -e ENABLE_INIT_DAEMON=false -d -v /Users/haixuanguo/Documents/usu/Spring2020/cs6665:/homework bde2020/spark-master:2.3.0-hadoop2.7 

# run it
docker exec -it spark-master bash

# interpreter in 【spark】 folder
# run spark, default scala
/spark/bin/spark-shell

# run pyspark
/spark/bin/pyspark

# use ipython
PYSPARK_DRIVER_PYTHON=ipython spark/bin/pyspark



```

# 2. count


```python
import string
import re
regex = re.compile('[%s]' % re.escape(string.punctuation))
lines = sc.textFile("../homework/HW1/pg100.txt")

def transform(text):
	s = str(text).lower()
	# return s.translate(None, string.punctuation)
    return regex.sub(' ', s)

count = lines.flatMap(lambda line: transform(line).split(" ")).filter(lambda l: l!='').map(lambda word: (word,1)).reduceByKey(lambda x,y: x+y).sortBy(lambda (k,v): -1*v)
    
count.take(10)
count.saveAsTextFile("../homework/HW1/output.csv")


```


