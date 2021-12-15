# Google Cloud Dataflow Template Pipelines

This fork is done to "fix" some state of the Google's original repo, since they don't maintain any tags or versions there.
The only part is changed so far is this README file.

It will just hold the instruction on how to build Google's templates in a way, Google does not do so far.

## Prerequisites
On your local machine you need to have:
* Java 8. With some newer versions build fails, but with Java 8 it works just fine. And make sure that `JAVA_HOME` is pointing to this version.
* Maven 3

I propose to use a kind of the same version convention in a bucket with built jos as Google does, so when you want to make a new build - place it in `gs://dataflow-staging-europe-west1-105942741667/templates/google/YYYY-MM-DD/Job_Name`

## PubsubToAvro in subscritpion mode
PubsubToAvro can be build in 2 ways:
* where you can define input topic, and job will create subscription automatically every time it's started - this one is being built by Google, but we don't want it.
* where you can define input subscription - this one we need. And since Google for some reason don't build it itself in their public bucket, the instruction below will guide how to do that.

In order to build new version and upload it to GCS simply run the following command (put the actual date inside!):
```
mvn compile exec:java \
-Dexec.mainClass=com.google.cloud.teleport.templates.PubsubToAvro \
-Dexec.cleanupDaemonThreads=false \
-Dexec.args=" \
--project=epidemic-data-infra \
--stagingLocation=gs://dataflow-staging-europe-west1-105942741667/templates/google/staging \
--tempLocation=gs://dataflow-staging-europe-west1-105942741667/templates/google/temp \
--templateLocation=gs://dataflow-staging-europe-west1-105942741667/templates/google/[DATE in YYYY-MM-DD]/PubsubToAvro \
--runner=DataflowRunner \
--region=europe-west1 \
--useSubscription=true"
```

And then just the the folder.
