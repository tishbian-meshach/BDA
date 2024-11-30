# Hadoop Word Count Example

This project demonstrates a basic word counting application using Hadoop MapReduce.

## Prerequisites

- Hadoop installed and configured
- Java Development Kit (JDK)
- Hadoop classpath set up correctly

## Setup

1. Create a new directory for your project:

   ```
   mkdir word_count
   cd word_count
   ```

2. Create a new file named `WordCount.java` and add the following code:

   ```java
   import java.io.IOException;
   import java.util.StringTokenizer;

   import org.apache.hadoop.io.IntWritable;
   import org.apache.hadoop.io.LongWritable;
   import org.apache.hadoop.io.Text;
   import org.apache.hadoop.mapreduce.Mapper;
   import org.apache.hadoop.mapreduce.Reducer;
   import org.apache.hadoop.conf.Configuration;
   import org.apache.hadoop.mapreduce.Job;
   import org.apache.hadoop.mapreduce.lib.input.TextInputFormat;
   import org.apache.hadoop.mapreduce.lib.output.TextOutputFormat;
   import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
   import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
   import org.apache.hadoop.fs.Path;

   public class WordCount {

       public static class Map extends Mapper<LongWritable, Text, Text, IntWritable> {
           private final static IntWritable one = new IntWritable(1);
           private Text word = new Text();

           public void map(LongWritable key, Text value, Context context)
                   throws IOException, InterruptedException {
               String line = value.toString();
               StringTokenizer tokenizer = new StringTokenizer(line);
               while (tokenizer.hasMoreTokens()) {
                   word.set(tokenizer.nextToken());
                   context.write(word, one);
               }
           }
       }

       public static class Reduce extends Reducer<Text, IntWritable, Text, IntWritable> {
           public void reduce(Text key, Iterable<IntWritable> values, Context context)
                   throws IOException, InterruptedException {
               int sum = 0;
               for (IntWritable value : values) {
                   sum += value.get();
               }
               context.write(key, new IntWritable(sum));
           }
       }

       public static void main(String[] args) throws Exception {
           Configuration conf = new Configuration();
           Job job = Job.getInstance(conf, "My Word Count Program");

           job.setJarByClass(WordCount.class);
           job.setMapperClass(Map.class);
           job.setReducerClass(Reduce.class);

           job.setOutputKeyClass(Text.class);
           job.setOutputValueClass(IntWritable.class);

           job.setInputFormatClass(TextInputFormat.class);
           job.setOutputFormatClass(TextOutputFormat.class);

           // Configure input and output paths
           FileInputFormat.addInputPath(job, new Path(args[0]));
           FileOutputFormat.setOutputPath(job, new Path(args[1]));

           // Delete the output path if it exists
           Path outputPath = new Path(args[1]);
           outputPath.getFileSystem(conf).delete(outputPath, true);

           // Exit after job completion
           System.exit(job.waitForCompletion(true) ? 0 : 1);
       }
   }
   ```

## Compilation and Execution

1. Compile the Java code:

   ```
   javac -classpath %HADOOP_HOME%\share\hadoop\common\*;%HADOOP_HOME%\share\hadoop\mapreduce\* WordCount.java
   ```

2. Create a JAR file:

   ```
   jar cvf WordCount.jar *.class
   ```

3. Prepare input data:
   Create a text file named `input.txt` with some sample text for word counting.

4. Create input directory in HDFS and upload the input file:

   ```
   hadoop fs -mkdir /input
   hadoop fs -put input.txt /input
   ```

5. Run the MapReduce job:

   ```
   hadoop jar C:\Downloads\word_count\WordCount.jar WordCount /input/input.txt /output
   ```

6. View the results:

   ```
   hadoop fs -cat /output/*
   ```

## How it Works

1. The **Mapper** class:
   - Takes input text and splits it into words
   - Emits each word with a count of 1
   - Key: word (Text), Value: 1 (IntWritable)

2. The **Reducer** class:
   - Receives all counts for each word
   - Sums up the counts
   - Emits the final count for each word

## Output Format

The output will be in the format:
```
word1    count1
word2    count2
...
```

## Notes

- The program automatically deletes the output directory if it exists
- Make sure you have the necessary permissions to create directories and files in HDFS
- The input text file can be any text document you want to analyze

## Troubleshooting

- If you encounter class path issues, verify your Hadoop installation and environment variables
- Check Hadoop logs for detailed error messages if the job fails
- Ensure the input file is properly formatted text file
