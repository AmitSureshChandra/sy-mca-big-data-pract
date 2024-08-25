```java
// WAP in Map Reduce for Intersection Operation.

public class IntersectionRunner
{
    public static void main( String[] args ) throws IOException, InterruptedException, ClassNotFoundException {
        JobConf conf = new JobConf();
        FileInputFormat.addInputPath(conf, new Path(args[0]));
        FileOutputFormat.setOutputPath(conf, new Path(args[1]));

        Job job = Job.getInstance(conf, "intersection");

        job.setJarByClass(IntersectionRunner.class);
        job.setMapperClass(IntersectionMapper.class);
        job.setCombinerClass(IntersectionCombiner.class);
        job.setReducerClass(IntersectionReducer.class);


        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(IntWritable.class);

        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);

        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}


public class IntersectionMapper extends Mapper<LongWritable, Text, Text, IntWritable> {

    private final static IntWritable one = new IntWritable(1);

    @Override
    protected void map(LongWritable key, Text value, Mapper<LongWritable, Text, Text, IntWritable>.Context context) throws IOException, InterruptedException {
        context.write(value, one);
    }
}


public class IntersectionCombiner extends Reducer<Text, IntWritable, Text, IntWritable> {

    Logger logger = Logger.getLogger(String.valueOf(IntersectionCombiner.class));

    @Override
    protected void reduce(Text key, Iterable<IntWritable> values, Reducer<Text, IntWritable, Text, IntWritable>.Context context) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
            sum += val.get();
        }

        logger.info(key.toString() + " : " + sum);
        context.write(key, new IntWritable(sum));
    }

}

public class IntersectionReducer extends Reducer<Text, IntWritable, Text, Text> {
    @Override
    protected void reduce(Text key, Iterable<IntWritable> values, Reducer<Text, IntWritable, Text, Text>.Context context) throws IOException, InterruptedException {
        int sum = 0;

        for (IntWritable value : values) {
            sum += value.get();
        }

        if(sum > 1) {
            context.write(key, new Text());
        }
    }
}
```


```bash

➜  ~ hdfs dfs -du /input
75  75  /input/input.txt
10  10  /input/union-input1.txt
10  10  /input/union-input2.txt

➜  ~ hdfs dfs -cat /input/union-input1.txt
some text

➜  ~ hdfs dfs -cat /input/union-input2.txt
some text

➜  ~ hdfs dfs -cat /input/input.txt
some text is here written for some reason to test the map reduce text here

➜  ~ hadoop jar /mnt/d/java-project/mapreduce-hadoop/target/mapreduce-hadoop-1.0-SNAPSHOT.jar com.github.AmitSurechChandra.intersection.IntersectionRunner /input /intersection-output
2024-08-25 21:26:09,632 INFO client.DefaultNoHARMFailoverProxyProvider: Connecting to ResourceManager at /0.0.0.0:8032
2024-08-25 21:26:10,262 WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
2024-08-25 21:26:10,312 INFO mapreduce.JobResourceUploader: Disabling Erasure Coding for path: /tmp/hadoop-yarn/staging/ak00029/.staging/job_1724599555417_0007
2024-08-25 21:26:10,815 INFO input.FileInputFormat: Total input files to process : 3
2024-08-25 21:26:11,779 INFO mapreduce.JobSubmitter: number of splits:3
2024-08-25 21:26:12,116 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1724599555417_0007
2024-08-25 21:26:12,117 INFO mapreduce.JobSubmitter: Executing with tokens: []
2024-08-25 21:26:12,379 INFO conf.Configuration: resource-types.xml not found
2024-08-25 21:26:12,380 INFO resource.ResourceUtils: Unable to find 'resource-types.xml'.
2024-08-25 21:26:12,505 INFO impl.YarnClientImpl: Submitted application application_1724599555417_0007
2024-08-25 21:26:12,552 INFO mapreduce.Job: The url to track the job: http://ak00029.:8088/proxy/application_1724599555417_0007/
2024-08-25 21:26:12,553 INFO mapreduce.Job: Running job: job_1724599555417_0007
2024-08-25 21:26:20,757 INFO mapreduce.Job: Job job_1724599555417_0007 running in uber mode : false
2024-08-25 21:26:20,759 INFO mapreduce.Job:  map 0% reduce 0%
2024-08-25 21:26:27,922 INFO mapreduce.Job:  map 100% reduce 0%
2024-08-25 21:26:33,996 INFO mapreduce.Job:  map 100% reduce 100%
2024-08-25 21:26:36,046 INFO mapreduce.Job: Job job_1724599555417_0007 completed successfully
2024-08-25 21:26:36,203 INFO mapreduce.Job: Counters: 54
        File System Counters
                FILE: Number of bytes read=119
                FILE: Number of bytes written=1235851
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=415
                HDFS: Number of bytes written=11
                HDFS: Number of read operations=14
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
                HDFS: Number of bytes read erasure-coded=0
        Job Counters
                Launched map tasks=3
                Launched reduce tasks=1
                Data-local map tasks=3
                Total time spent by all maps in occupied slots (ms)=14685
                Total time spent by all reduces in occupied slots (ms)=3651
                Total time spent by all map tasks (ms)=14685
                Total time spent by all reduce tasks (ms)=3651
                Total vcore-milliseconds taken by all map tasks=14685
                Total vcore-milliseconds taken by all reduce tasks=3651
                Total megabyte-milliseconds taken by all map tasks=15037440
                Total megabyte-milliseconds taken by all reduce tasks=3738624
        Map-Reduce Framework
                Map input records=3
                Map output records=3
                Map output bytes=107
                Map output materialized bytes=131
                Input split bytes=320
                Combine input records=3
                Combine output records=3
                Reduce input groups=2
                Reduce shuffle bytes=131
                Reduce input records=3
                Reduce output records=1
                Spilled Records=6
                Shuffled Maps =3
                Failed Shuffles=0
                Merged Map outputs=3
                GC time elapsed (ms)=158
                CPU time spent (ms)=3850
                Physical memory (bytes) snapshot=1181454336
                Virtual memory (bytes) snapshot=11024314368
                Total committed heap usage (bytes)=837812224
                Peak Map Physical memory (bytes)=324067328
                Peak Map Virtual memory (bytes)=2754940928
                Peak Reduce Physical memory (bytes)=222560256
                Peak Reduce Virtual memory (bytes)=2764308480
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters
                Bytes Read=95
        File Output Format Counters
                Bytes Written=11

➜  ~ hdfs dfs -du /intersection-output
0   0   /intersection-output/_SUCCESS
11  11  /intersection-output/part-r-00000

➜  ~ hdfs dfs -cat /intersection-output/part-r-00000
some text


```
