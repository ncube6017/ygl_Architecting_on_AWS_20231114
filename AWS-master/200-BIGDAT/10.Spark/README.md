# 모듈10: Amazon EMR의 Apache Spark

1. 인메모리 분석 동기

2. Apache Spark

3. Spark 프로그래밍 모델

4. Spark 라이브러리

5. Spark 사용

6. Amazon EMR의 Spark 이점

--- 

# Demo


```bash
touch hello.txt
echo "hello world hello spark" >> hello.txt
aws s3 mb s3://hello-spark
aws s3 cp hello.txt s3://hello-spark/
spark-shell
```

```scala
# RDD
val file = sc.textFile("s3://hello-spark/hello.txt")

val counts = file.
  flatMap(line => line.
    toLowerCase().
    replace(".", " ").
    replace(",", " ").
    split(" ")).
  map(word => (word, 1L)).
  reduceByKey(_ + _)

counts.collect.foreach(println)
counts.toDebugString
# counts.collect().sortBy(wc => -wc._2)

# DataFrame
val file = spark.read.textFile("s3://hello-spark/hello.txt")
file.show()

val peopleDF = spark.sparkContext.
  textFile("s3://hello-spark/hello.txt").
  flatMap(line => line.
    toLowerCase().
    replace(".", " ").
    replace(",", " ").
    split(" ")).
  map(word => (word, 1L)).
  reduceByKey(_ + _)

peopleDF.toDF().show()
// Register the DataFrame as a temporary view
peopleDF.createOrReplaceTempView("people")
```