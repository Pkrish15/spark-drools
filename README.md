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
14) To stop the deletion of cluster, We have to continously run the spark-job to maintain or else, we can use spring-boot to monitor the cluster progress. <br>
15) Currently working on the 14th Point (ansible-playbook), Almost done! Will check in with additional code changes.<br>

