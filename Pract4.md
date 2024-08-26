```java 
// WAP in Map Reduce for Matrix Multiplication.

public class MatrixRunner {
    public static void main(String[] args) throws IOException, InterruptedException, ClassNotFoundException {
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf, "Matrix Multiplication");
        job.setJarByClass(MatrixRunner.class);
        job.setMapperClass(MatrixMapper.class);
        job.setReducerClass(MatrixReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        FileInputFormat.addInputPath(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);    }
}


public class MatrixMapper extends Mapper<Object, Text, Text, Text> {

    private Text outKey = new Text();
    private Text outValue = new Text();

    @Override
    protected void map(Object key, Text value, Mapper<Object, Text, Text, Text>.Context context) throws IOException, InterruptedException {
        String[] tokens = value.toString().split("\\s+");
        String matrixName = tokens[0];
        int row = Integer.parseInt(tokens[1]);
        int col = Integer.parseInt(tokens[2]);
        int cellValue = Integer.parseInt(tokens[3]);

        if (matrixName.equals("A")) {
            for (int k = 0; k <= 1; k++) {
                outKey.set(row + "," + k);
                outValue.set(matrixName + "," + col + "," + cellValue);
                context.write(outKey, outValue);
            }
        } else if (matrixName.equals("B")) {
            for (int i = 0; i <= 1; i++) {
                outKey.set(i + "," + col);
                outValue.set(matrixName + "," + row + "," + cellValue);
                context.write(outKey, outValue);
            }
        }
    }
}


public class MatrixReducer extends Reducer<Text, Text, Text, IntWritable> {

    private IntWritable result = new IntWritable();

    @Override
    protected void reduce(Text key, Iterable<Text> values, Reducer<Text, Text, Text, IntWritable>.Context context) throws IOException, InterruptedException {

        int sum = 0;
        int[] aValues = new int[2];
        int[] bValues = new int[2];

        for (Text val : values) {
            String[] tokens = val.toString().split(",");
            String matrixName = tokens[0];
            int index = Integer.parseInt(tokens[1]);
            int cellValue = Integer.parseInt(tokens[2]);

            if (matrixName.equals("A")) {
                aValues[index] = cellValue;
            } else if (matrixName.equals("B")) {
                bValues[index] = cellValue;
            }
        }

        for (int i = 0; i < 2; i++) {
            sum += aValues[i] * bValues[i];
        }

        result.set(sum);
        context.write(key, result);
    }
}

```


```bash

➜  ~ hdfs dfs -du /input
32  32  /input/matrix-A.txt
32  32  /input/matrix-B.txt

➜  ~ hdfs dfs -cat /input/matrix-A.txt
A 0 0 1
A 0 1 2
A 1 0 3
A 1 1 4
➜  ~ hdfs dfs -cat /input/matrix-B.txt
B 0 0 5
B 0 1 6
B 1 0 7
B 1 1 8

➜  ~ hadoop jar /mnt/d/java-project/mapreduce-hadoop/target/mapreduce-hadoop-1.0-SNAPSHOT.jar com.github.AmitSurechChandra.matrix.MatrixRunner /input /output
2024-08-26 21:05:03,918 INFO client.DefaultNoHARMFailoverProxyProvider: Connecting to ResourceManager at /0.0.0.0:8032
2024-08-26 21:05:04,194 WARN mapreduce.JobResourceUploader: Hadoop command-line option parsing not performed. Implement the Tool interface and execute your application with ToolRunner to remedy this.
2024-08-26 21:05:04,213 INFO mapreduce.JobResourceUploader: Disabling Erasure Coding for path: /tmp/hadoop-yarn/staging/ak00029/.staging/job_1724681747117_0008
2024-08-26 21:05:04,500 INFO input.FileInputFormat: Total input files to process : 2
2024-08-26 21:05:04,985 INFO mapreduce.JobSubmitter: number of splits:2
2024-08-26 21:05:05,153 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1724681747117_0008
2024-08-26 21:05:05,153 INFO mapreduce.JobSubmitter: Executing with tokens: []
2024-08-26 21:05:05,294 INFO conf.Configuration: resource-types.xml not found
2024-08-26 21:05:05,295 INFO resource.ResourceUtils: Unable to find 'resource-types.xml'.
2024-08-26 21:05:05,345 INFO impl.YarnClientImpl: Submitted application application_1724681747117_0008
2024-08-26 21:05:05,371 INFO mapreduce.Job: The url to track the job: http://ak00029.:8088/proxy/application_1724681747117_0008/
2024-08-26 21:05:05,372 INFO mapreduce.Job: Running job: job_1724681747117_0008
2024-08-26 21:05:10,449 INFO mapreduce.Job: Job job_1724681747117_0008 running in uber mode : false
2024-08-26 21:05:10,451 INFO mapreduce.Job:  map 0% reduce 0%
2024-08-26 21:05:15,544 INFO mapreduce.Job:  map 100% reduce 0%
2024-08-26 21:05:19,575 INFO mapreduce.Job:  map 100% reduce 100%
2024-08-26 21:05:20,601 INFO mapreduce.Job: Job job_1724681747117_0008 completed successfully
2024-08-26 21:05:20,700 INFO mapreduce.Job: Counters: 54
        File System Counters
                FILE: Number of bytes read=198
                FILE: Number of bytes written=925226
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=274
                HDFS: Number of bytes written=28
                HDFS: Number of read operations=11
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
                HDFS: Number of bytes read erasure-coded=0
        Job Counters
                Launched map tasks=2
                Launched reduce tasks=1
                Data-local map tasks=2
                Total time spent by all maps in occupied slots (ms)=4812
                Total time spent by all reduces in occupied slots (ms)=2031
                Total time spent by all map tasks (ms)=4812
                Total time spent by all reduce tasks (ms)=2031
                Total vcore-milliseconds taken by all map tasks=4812
                Total vcore-milliseconds taken by all reduce tasks=2031
                Total megabyte-milliseconds taken by all map tasks=4927488
                Total megabyte-milliseconds taken by all reduce tasks=2079744
        Map-Reduce Framework
                Map input records=8
                Map output records=16
                Map output bytes=160
                Map output materialized bytes=204
                Input split bytes=210
                Combine input records=0
                Combine output records=0
                Reduce input groups=4
                Reduce shuffle bytes=204
                Reduce input records=16
                Reduce output records=4
                Spilled Records=32
                Shuffled Maps =2
                Failed Shuffles=0
                Merged Map outputs=2
                GC time elapsed (ms)=64
                CPU time spent (ms)=1360
                Physical memory (bytes) snapshot=837378048
                Virtual memory (bytes) snapshot=8262877184
                Total committed heap usage (bytes)=601882624
                Peak Map Physical memory (bytes)=313978880
                Peak Map Virtual memory (bytes)=2752573440
                Peak Reduce Physical memory (bytes)=211447808
                Peak Reduce Virtual memory (bytes)=2758582272
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters
                Bytes Read=64
        File Output Format Counters
                Bytes Written=28

➜  ~ hdfs dfs -du /output
0   0   /output/_SUCCESS
28  28  /output/part-r-00000

➜  ~ hdfs dfs -cat /output/part-r-00000
0,0     19
0,1     22
1,0     43
1,1     50

```
