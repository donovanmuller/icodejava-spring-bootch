# Spring Boot Batch

This project is a demo from my I Code Java 2016 talk entitled "Stream is the new batch"

The application is a trivial Spring Boot Batch application that reads a file and capitalises
each line, printing the result to the log.

## Running the batch

Start the Spring Boot Batch application with:

```
$ ./mvnw spring-boot:run
```

First we will copy over the file that our batch application will process:

```
$ mkdir -p /tmp/icodejava
$ cp batch/input.txt /tmp/icodejava 
```

Now we can trigger the batch processing with a `POST` to the trigger resource:
 
```
$ curl -X POST -H "Content-Type: text/plain" -d '/tmp/icodejava/input.txt' http://localhost:8080/batch
```

The `curl` call should block and you'll see the following in the application log:

```
...  INFO 28908 --- [nio-8080-exec-1] i.code.java.batch.BatchResource          : Triggering batch job for: /tmp/icodejava/input.txt
...  INFO 28908 --- [nio-8080-exec-1] o.s.b.c.l.support.SimpleJobLauncher      : Job: [SimpleJob: [name=uppercase]] launched with the following parameters: [{filename=/tmp/icodejava/input.txt}]
...  INFO 28908 --- [nio-8080-exec-1] o.s.batch.core.job.SimpleStepHandler     : Executing step: [capitalize]
...  INFO 28908 --- [nio-8080-exec-1] i.code.java.batch.JobConfiguration       : Oh my word -> I
...  INFO 28908 --- [nio-8080-exec-1] i.code.java.batch.JobConfiguration       : Oh my word -> CODE
...  INFO 28908 --- [nio-8080-exec-1] i.code.java.batch.JobConfiguration       : Oh my word -> JAVA
...  INFO 28908 --- [nio-8080-exec-1] i.code.java.batch.JobConfiguration       : Oh my word -> 2016
...  INFO 28908 --- [nio-8080-exec-1] i.code.java.batch.JobConfiguration       : Oh my word -> LEGACY
...  INFO 28908 --- [nio-8080-exec-1] i.code.java.batch.JobConfiguration       : Oh my word -> BATCH
...  INFO 28908 --- [nio-8080-exec-1] i.code.java.batch.JobConfiguration       : Oh my word -> DEMO
...  INFO 28908 --- [nio-8080-exec-1] o.s.b.c.l.support.SimpleJobLauncher      : Job: [SimpleJob: [name=uppercase]] completed with the following parameters: [{filename=/tmp/icodejava/input.txt}] and the following status: [COMPLETED]
...  INFO 28908 --- [nio-8080-exec-1] i.code.java.batch.BatchResource          : Started job: JobExecution: id=1, version=2, startTime=..., endTime=..., lastUpdated=..., status=COMPLETED, exitStatus=exitCode=COMPLETED;exitDescription=, job=[JobInstance: id=1, version=0, Job=[uppercase]], jobParameters=[{filename=/tmp/icodejava/input.txt}]
```

