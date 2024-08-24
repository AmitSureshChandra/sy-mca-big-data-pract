```java
// Map Reduce for Union Operation.

public class UnionRunner
{
    public static void main( String[] args ) throws IOException {
        JobConf conf = new JobConf(UnionRunner.class);
        conf.setJobName("Union");
        conf.setOutputKeyClass(Text.class);
        conf.setOutputValueClass(Text.class);
        conf.setMapperClass(UnionMapper.class);
        conf.setCombinerClass(UnionReducer.class);
        conf.setReducerClass(UnionReducer.class);
        conf.setInputFormat(TextInputFormat.class);
        conf.setOutputFormat(TextOutputFormat.class);
        FileInputFormat.setInputPaths(conf,new Path(args[0]));
        FileOutputFormat.setOutputPath(conf,new Path(args[1]));
        JobClient.runJob(conf);
    }
}

public class UnionMapper extends MapReduceBase implements Mapper<Object, Text, Text, Text> {

    @Override
    public void map(Object key, Text value, OutputCollector<Text, Text> output, Reporter reporter) throws IOException {
        output.collect(value, value);
    }
}

public class UnionReducer extends MapReduceBase implements Reducer<Text, Text, Text, Text> {
    @Override
    public void reduce(Text key, Iterator<Text> values, OutputCollector<Text, Text> output, Reporter reporter) throws IOException {
        output.collect(key, new Text(""));
    }
}


```

```bash
➜  ~ hdfs dfs -du /input
75  75  /input/input.txt
10  10  /input/union-input1.txt
14  14  /input/union-input2.txt

➜  ~ hdfs dfs -cat /input/input.txt
some text is here written for some reason to test the map reduce text here

➜  ~ hdfs dfs -cat /input/union-input1.txt
A
B
C
D
E

➜  ~ hdfs dfs -cat /input/union-input2.txt
F
G
H
I
J
K
L

➜  ~ hadoop jar /mnt/d/java-project/mapreduce-hadoop/target/mapreduce-hadoop-1.0-SNAPSHOT.jar com.github.AmitSurechChandra.union.UnionRunner /input /union-output
2024-08-24 06:19:07,163 INFO client.DefaultNoHARMFailoverProxyProvider: Connecting to ResourceManager at /0.0.0.0:8032
2024-08-24 06:19:07,381 INFO client.DefaultNoHARMFailoverProxyProvider: Connecting to ResourceManager at /0.0.0.0:8032
2024-08-24 06:19:07,731 WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
2024-08-24 06:19:07,771 INFO mapreduce.JobResourceUploader: Disabling Erasure Coding for path: /tmp/hadoop-yarn/staging/ak00029/.staging/job_1724460015896_0003
2024-08-24 06:19:08,201 INFO mapred.FileInputFormat: Total input files to process : 3
2024-08-24 06:19:08,285 INFO mapreduce.JobSubmitter: number of splits:4
2024-08-24 06:19:08,634 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1724460015896_0003
2024-08-24 06:19:08,634 INFO mapreduce.JobSubmitter: Executing with tokens: []
2024-08-24 06:19:08,882 INFO conf.Configuration: resource-types.xml not found
2024-08-24 06:19:08,883 INFO resource.ResourceUtils: Unable to find 'resource-types.xml'.
2024-08-24 06:19:08,987 INFO impl.YarnClientImpl: Submitted application application_1724460015896_0003
2024-08-24 06:19:09,039 INFO mapreduce.Job: The url to track the job: http://ak00029.:8088/proxy/application_1724460015896_0003/
2024-08-24 06:19:09,041 INFO mapreduce.Job: Running job: job_1724460015896_0003
2024-08-24 06:19:17,210 INFO mapreduce.Job: Job job_1724460015896_0003 running in uber mode : false
2024-08-24 06:19:17,211 INFO mapreduce.Job:  map 0% reduce 0%
2024-08-24 06:19:25,441 INFO mapreduce.Job:  map 100% reduce 0%
2024-08-24 06:19:31,519 INFO mapreduce.Job:  map 100% reduce 100%
2024-08-24 06:19:32,588 INFO mapreduce.Job: Job job_1724460015896_0003 completed successfully
2024-08-24 06:19:32,732 INFO mapreduce.Job: Counters: 55
        File System Counters
                FILE: Number of bytes read=144
                FILE: Number of bytes written=1544531
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=495
                HDFS: Number of bytes written=112
                HDFS: Number of read operations=17
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
                HDFS: Number of bytes read erasure-coded=0
        Job Counters
                Killed map tasks=1
                Launched map tasks=4
                Launched reduce tasks=1
                Data-local map tasks=4
                Total time spent by all maps in occupied slots (ms)=21008
                Total time spent by all reduces in occupied slots (ms)=3890
                Total time spent by all map tasks (ms)=21008
                Total time spent by all reduce tasks (ms)=3890
                Total vcore-milliseconds taken by all map tasks=21008
                Total vcore-milliseconds taken by all reduce tasks=3890
                Total megabyte-milliseconds taken by all map tasks=21512192
                Total megabyte-milliseconds taken by all reduce tasks=3983360
        Map-Reduce Framework
                Map input records=13
                Map output records=13
                Map output bytes=198
                Map output materialized bytes=162
                Input split bytes=370
                Combine input records=0
                Combine output records=13
                Reduce input groups=13
                Reduce shuffle bytes=162
                Reduce input records=0
                Reduce output records=13
                Spilled Records=26
                Shuffled Maps =4
                Failed Shuffles=0
                Merged Map outputs=4
                GC time elapsed (ms)=239
                CPU time spent (ms)=4890
                Physical memory (bytes) snapshot=1441091584
                Virtual memory (bytes) snapshot=13768306688
                Total committed heap usage (bytes)=999292928
                Peak Map Physical memory (bytes)=315031552
                Peak Map Virtual memory (bytes)=2758311936
                Peak Reduce Physical memory (bytes)=209776640
                Peak Reduce Virtual memory (bytes)=2758701056
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters
                Bytes Read=125
        File Output Format Counters
                Bytes Written=112

➜  ~ hdfs dfs -du /union-output
0    0    /union-output/_SUCCESS
112  112  /union-output/part-00000

➜  ~ hdfs dfs -cat /union-output/part-00000
A
B
C
D
E
F
G
H
I
J
K
L
some text is here written for some reason to test the map reduce text here
```

