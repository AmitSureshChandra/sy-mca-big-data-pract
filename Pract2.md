Pract2 : WAP in Map Reduce for Word Count Operation.

```java
public class WC_Runner
{
    public static void main( String[] args ) throws IOException {
        JobConf conf = new JobConf(WC_Runner.class);
        conf.setJobName("WordCount");
        conf.setOutputKeyClass(Text.class);
        conf.setOutputValueClass(IntWritable.class);
        conf.setMapperClass(WC_Mapper.class);
        conf.setCombinerClass(WC_Reducer.class);
        conf.setReducerClass(WC_Reducer.class);
        conf.setInputFormat(TextInputFormat.class);
        conf.setOutputFormat(TextOutputFormat.class);
        FileInputFormat.setInputPaths(conf,new Path(args[0]));
        FileOutputFormat.setOutputPath(conf,new Path(args[1]));
        JobClient.runJob(conf);
    }
}

```

```java
public class WC_Mapper extends MapReduceBase implements Mapper<LongWritable, Text,Text, IntWritable> {

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();


    @Override
    public void map(LongWritable longWritable, Text text, OutputCollector<Text, IntWritable> outputCollector, Reporter reporter) throws IOException {
        String line = text.toString();
        StringTokenizer tokenizer = new StringTokenizer(line);
        while (tokenizer.hasMoreTokens()){
            word.set(tokenizer.nextToken());
            outputCollector.collect(word, one);
        }
    }
}
```

```java
public class WC_Reducer extends MapReduceBase implements Reducer<Text, IntWritable, Text,IntWritable> {
    @Override
    public void reduce(Text text, Iterator<IntWritable> iterator, OutputCollector<Text, IntWritable> outputCollector, Reporter reporter) throws IOException {
        int sum = 0;

        while (iterator.hasNext()) {
            sum += iterator.next().get();
        }

        outputCollector.collect(text, new IntWritable(sum));
    }
}
```

![image](https://github.com/user-attachments/assets/55d83f2a-9943-4d82-86c4-32e8cce0259f)

```cmd
➜  ~ hdfs dfs -mkdir /input
➜  ~ hdfs dfs -put ./input.txt /input

➜  ~ hadoop jar /mnt/d/java-project/mapreduce-hadoop/target/mapreduce-hadoop-1.0-SNAPSHOT.jar com.github.AmitSurec
hChandra.WC_Runner /input/input.txt /output
2024-08-19 18:15:55,143 INFO client.DefaultNoHARMFailoverProxyProvider: Connecting to ResourceManager at /0.0.0.0:8032
2024-08-19 18:15:55,246 INFO client.DefaultNoHARMFailoverProxyProvider: Connecting to ResourceManager at /0.0.0.0:8032
2024-08-19 18:15:55,466 WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
2024-08-19 18:15:55,482 INFO mapreduce.JobResourceUploader: Disabling Erasure Coding for path: /tmp/hadoop-yarn/staging/ak00029/.staging/job_1724069726867_0006
2024-08-19 18:15:55,733 INFO mapred.FileInputFormat: Total input files to process : 1
2024-08-19 18:15:55,789 INFO mapreduce.JobSubmitter: number of splits:2
2024-08-19 18:15:55,939 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1724069726867_0006
2024-08-19 18:15:55,940 INFO mapreduce.JobSubmitter: Executing with tokens: []
2024-08-19 18:15:56,081 INFO conf.Configuration: resource-types.xml not found
2024-08-19 18:15:56,082 INFO resource.ResourceUtils: Unable to find 'resource-types.xml'.
2024-08-19 18:15:56,143 INFO impl.YarnClientImpl: Submitted application application_1724069726867_0006
2024-08-19 18:15:56,166 INFO mapreduce.Job: The url to track the job: http://ak00029.:8088/proxy/application_1724069726867_0006/
2024-08-19 18:15:56,167 INFO mapreduce.Job: Running job: job_1724069726867_0006
2024-08-19 18:16:01,256 INFO mapreduce.Job: Job job_1724069726867_0006 running in uber mode : false
2024-08-19 18:16:01,257 INFO mapreduce.Job:  map 0% reduce 0%
2024-08-19 18:16:06,338 INFO mapreduce.Job:  map 100% reduce 0%
2024-08-19 18:16:10,378 INFO mapreduce.Job:  map 100% reduce 100%
2024-08-19 18:16:11,397 INFO mapreduce.Job: Job job_1724069726867_0006 completed successfully
2024-08-19 18:16:11,488 INFO mapreduce.Job: Counters: 54
        File System Counters
                FILE: Number of bytes read=138
                FILE: Number of bytes written=926768
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=291
                HDFS: Number of bytes written=84
                HDFS: Number of read operations=11
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
                HDFS: Number of bytes read erasure-coded=0
        Job Counters
                Launched map tasks=2
                Launched reduce tasks=1
                Data-local map tasks=2
                Total time spent by all maps in occupied slots (ms)=4762
                Total time spent by all reduces in occupied slots (ms)=2039
                Total time spent by all map tasks (ms)=4762
                Total time spent by all reduce tasks (ms)=2039
                Total vcore-milliseconds taken by all map tasks=4762
                Total vcore-milliseconds taken by all reduce tasks=2039
                Total megabyte-milliseconds taken by all map tasks=4876288
                Total megabyte-milliseconds taken by all reduce tasks=2087936
        Map-Reduce Framework
                Map input records=1
                Map output records=15
                Map output bytes=135
                Map output materialized bytes=144
                Input split bytes=178
                Combine input records=15
                Combine output records=12
                Reduce input groups=12
                Reduce shuffle bytes=144
                Reduce input records=12
                Reduce output records=12
                Spilled Records=24
                Shuffled Maps =2
                Failed Shuffles=0
                Merged Map outputs=2
                GC time elapsed (ms)=56
                CPU time spent (ms)=1740
                Physical memory (bytes) snapshot=856842240
                Virtual memory (bytes) snapshot=8277446656
                Total committed heap usage (bytes)=601882624
                Peak Map Physical memory (bytes)=327217152
                Peak Map Virtual memory (bytes)=2757468160
                Peak Reduce Physical memory (bytes)=215216128
                Peak Reduce Virtual memory (bytes)=2769879040
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters
                Bytes Read=113
        File Output Format Counters
                Bytes Written=84

➜  ~ cat input.txt
some text is here written for some reason to test the map reduce text here

```

![image](https://github.com/user-attachments/assets/62fab25e-6d0f-49bb-8278-9a1baa170ee5)

![image](https://github.com/user-attachments/assets/d9822e38-fb70-4e3b-b28b-6bf882da0afa)


