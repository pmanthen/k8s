# Batch/Cron Jobs on Kubernetes
Introduction: Some Workload will have background or long-running processes that need to run periodically on given schedule based on business requirements. Kubernetes cronjob capability will be utilized for the same. Persistence layer will be outside the scope of cron jobs, so that jobs are stateless and scale based on the data volume, time-window allowed configurations for the cron job. Jobs should be Idempotent.
Design Details:

    Batch Job hosting on Kubernetes ecosystem will be, in general, have following sequences

Have the business/processing logic implemented by connecting to required data sources and publish the docker image to ECR

We need to come up with cron Schedule expression based on the convention followed(added screen grab for quick reference below)

Sample:

apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            args:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
          
  Parallel Execution:

 Job Spec section can have completions or parallelism can be utilized

 

Job Termination:

restartPolicy of Never or onFailure can be utilized based on the job use case

 
