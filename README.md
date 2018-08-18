# spark-drools

1) This Project will show how to use the spark job to fire the rules.<br>
2) Simple Credit Approval logic, if the Applicant Credit score is greater than 600, fire the rules (approve the loan). <br>
3) Approval.drl (drool file) holds the business logic.<br>
4) I have checked in the sucessful output.log. <br>
5) Project can be easily imported as an Maven Project and fitted to be running in any ide.
6) Idea is based on this video https://www.redhat.com/en/about/videos/red-hat-consulting-decision-manager-and-apache-spark-integration. <br>

# Deployment in OpenShift - Oshinko Cluster

7) oc new-project demo <br>
8) oc create -f https://raw.githubusercontent.com/Pkrish15/spark-drools/master/resources.yaml <br>
9) oc new-app oshinko-webui <br>
10) oc get routes <br>
11) oc new-app --template oshinko-java-spark-build-dc \
    -p APPLICATION_NAME=spark-drools \
    -p APP_MAIN_CLASS=com.redhat.gpte.App \
    -p GIT_URI=https://github.com/Pkrish15/spark-drools \
    -p APP_FILE=spark-drools.jar       <br><br>

12) You can find the Spark-Cluster Logs with Output as *"Number of Applicant Approved:5"* <br> 
13) Please note the cluster will immediately terminates, once the job completes. You can observe the logs in the Openshift WebUI console.<br>


# Why you need Apache Spark for this usecase? Can't be a simple drools application or RHDM?
16) Ofcourse, we can use the RedHatDecisionManager UI to upload the rules and can be dealth with any UI framework to display. <br><br>
17) Apache Spark Performs sequences of Operations like Inputting the data -> Filter the data -> Tag the Data (Business Logics like loan defaulters ) -> Model -> as the useful business data. Hence huge amount of Java Code (rule logic)in tagging the data for writing/cleansing the data for different transactions and scenarios which is not necessary.<br><br>
18) Instead you can use the RedHatDecisionManager which completes the seperates the business logic with the application scope and make the clear abstractions with business rules and application logic. Developer can easily maintain these rules in RedHatDecision Manager.<br><br>
19) Apart from these Spark Provides several performance benefits by handling and parallelly processing huge amount of data and also in near-realtime transactions.<br><br>
20) Whenever the RedhatDecisionManager Admin Pushes the rules, we can make it available as a decent data through spark job with UI.<br><br>

# How can it be performed?
21) Please check the code which is very simple and self explanatory.<br> <br>

# What are the important concepts used in Apache Spark?
22) BroadCast Variables - Advanced Concept in Spark.<br>
23) SparkContext Parallelization - Beginner Level.<br>

# BroadCast Variables in Spark
24) Where "m" is the "rules" which is shared/broadcasted across the nodes.<br>
![alt text](https://github.com/Pkrish15/spark-drools/blob/master/Screenshot%20from%202018-06-22%2014-42-22.png)<br>

# When to use BroadCast Variables in Spark
25) Before running each tasks on the available executors, Spark computes the task’s closure. The closure is those variables and methods which must be visible for the executor to perform its computations on the RDD.<br>

26) If you have huge array that is accessed from Spark Closures, for example some reference data, this array will be shipped to each spark node with closure. <br>
27) For example if you have 10 nodes cluster with 100 partitions (10 partitions per node), this Array will be distributed at least 100 times (10 times to each node).<br>
28) If you use broadcast it will be distributed once per node using efficient p2p protocol.<br>

# Important Point to use BroadCast Variables.
29) Once we broadcasted the value to the nodes, we shouldn’t make changes to its value to make sure each node have exact same copy of data. The modified value might be sent to another node later that would give unexpected results.<br>






