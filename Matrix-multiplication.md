# Hadoop Matrix Multiplication with MapReduce

This project demonstrates how to perform matrix multiplication using Hadoop MapReduce.

## Prerequisites

- Hadoop installed and configured
- Java Development Kit (JDK)
- Hadoop classpath set up correctly

## Setup

1. Create a new directory for your project:

   ```
   mkdir matrix_multiplication
   cd matrix_multiplication
   ```

2. Create a new file named `MatrixMultiplication.java` in the `matrix_multiplication` directory.

3. Copy and paste the following code into `MatrixMultiplication.java`:

   ```java
   // MatrixMultiplication.java
   import org.apache.hadoop.conf.Configuration;
   import org.apache.hadoop.fs.Path;
   import org.apache.hadoop.io.Text;
   import org.apache.hadoop.mapreduce.Job;
   import org.apache.hadoop.mapreduce.Mapper;
   import org.apache.hadoop.mapreduce.Reducer;
   import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
   import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
   import java.io.IOException;

   public class MatrixMultiplication {

       public static class MatrixMapper extends Mapper<Object, Text, Text, Text> {
           public void map(Object key, Text value, Context context) throws IOException, InterruptedException {
               String line = value.toString();
               String[] parts = line.split(",");
               if (parts.length == 4) {
                   String matrix = parts[0];
                   int row = Integer.parseInt(parts[1]);
                   int col = Integer.parseInt(parts[2]);
                   int val = Integer.parseInt(parts[3]);

                   if (matrix.equals("A")) {
                       for (int i = 0; i < 10; i++) { // Assuming matrix B has 10 columns
                           context.write(new Text(row + "," + i), new Text("A," + col + "," + val));
                       }
                   } else if (matrix.equals("B")) {
                       for (int i = 0; i < 10; i++) { // Assuming matrix A has 10 rows
                           context.write(new Text(i + "," + col), new Text("B," + row + "," + val));
                       }
                   }
               }
           }
       }

       public static class MatrixReducer extends Reducer<Text, Text, Text, Text> {
           public void reduce(Text key, Iterable<Text> values, Context context) throws IOException, InterruptedException {
               int[] A = new int[10];
               int[] B = new int[10];

               for (Text val : values) {
                   String[] parts = val.toString().split(",");
                   if (parts[0].equals("A")) {
                       A[Integer.parseInt(parts[1])] = Integer.parseInt(parts[2]);
                   } else if (parts[0].equals("B")) {
                       B[Integer.parseInt(parts[1])] = Integer.parseInt(parts[2]);
                   }
               }

               int result = 0;
               for (int i = 0; i < 10; i++) {
                   result += A[i] * B[i];
               }

               if (result != 0) {
                   context.write(key, new Text(Integer.toString(result)));
               }
           }
       }

       public static void main(String[] args) throws Exception {
           Configuration conf = new Configuration();
           Job job = Job.getInstance(conf, "matrix multiplication");
           job.setJarByClass(MatrixMultiplication.class);
           job.setMapperClass(MatrixMapper.class);
           job.setReducerClass(MatrixReducer.class);
           job.setOutputKeyClass(Text.class);
           job.setOutputValueClass(Text.class);
           FileInputFormat.addInputPath(job, new Path(args[0]));
           FileOutputFormat.setOutputPath(job, new Path(args[1]));
           System.exit(job.waitForCompletion(true) ? 0 : 1);
       }
   }
   ```

## Compilation and Execution

1. Compile the Java code:

   ```
   javac -classpath %HADOOP_HOME%\share\hadoop\common\*;%HADOOP_HOME%\share\hadoop\mapreduce\* MatrixMultiplication.java
   ```

2. Create a JAR file:

   ```
   jar cvf MatrixMultiplication.jar *.class
   ```

3. Prepare input data:
   Create a file named `input.txt` with your matrix data in the following format:

   ```
   A,0,0,1
   A,0,1,2
   B,0,0,3
   B,1,0,4
   ```

4. Create input and output directories in HDFS:

   ```
   hadoop fs -mkdir /input
   hadoop fs -put input.txt /input
   ```

5. Run the MapReduce job:

   ```
   hadoop jar C:\Users\wwwst\Downloads\matrix_multiplication\MatrixMultiplication.jar MatrixMultiplication /input/input.txt /output
   ```

6. View the results:

   ```
   hadoop fs -cat /output/*
   ```

## Notes

- This implementation assumes that matrix A has 10 rows and matrix B has 10 columns. Adjust these values in the code if your matrices have different dimensions.
- The input format is crucial. Each line should represent a single element of a matrix in the format: `MatrixName,RowIndex,ColumnIndex,Value`.
- The output will be in the format: `RowIndex,ColumnIndex Value`, representing the elements of the resulting matrix.

## Troubleshooting

- If you encounter class path issues, ensure that your Hadoop installation is correctly set up and that the `HADOOP_HOME` environment variable is properly defined.
- Make sure you have the necessary permissions to create directories and files in HDFS.
- If the job fails, check the Hadoop logs for more detailed error messages.
