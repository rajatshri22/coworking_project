Coworking Space Service Extension
The Coworking Space Service is a set of APIs that enables users to request one-time tokens and administrators to authorize access to a coworking space. This service follows a microservice pattern and the APIs are split into distinct services that can be deployed and managed independently of one another.

For this project, you are a DevOps engineer who will be collaborating with a team that is building an API for business analysts. The API provides business analysts basic analytics data on user activity in the service. The application they provide you functions as expected locally and you are expected to help build a pipeline to deploy it in Kubernetes.

Getting Started
Dependencies
Local Environment
Python Environment - run Python 3.6+ applications and install Python dependencies via pip
Docker CLI - build and run Docker images locally
kubectl - run commands against a Kubernetes cluster
helm - apply Helm Charts to a Kubernetes cluster
Remote Resources
AWS CodeBuild - build Docker images remotely
AWS ECR - host Docker images
Kubernetes Environment with AWS EKS - run applications in k8s
AWS CloudWatch - monitor activity and logs in EKS
GitHub - pull and clone code
Setup
1. Configure a Database
Set up a Postgres database using a Helm Chart.

Set up Bitnami Repo
helm repo add <REPO_NAME> https://charts.bitnami.com/bitnami
Install PostgreSQL Helm Chart
helm install <SERVICE_NAME> <REPO_NAME>/postgresql
This should set up a Postgre deployment at <SERVICE_NAME>-postgresql.default.svc.cluster.local in your Kubernetes cluster. You can verify it by running kubectl svc

By default, it will create a username postgres. The password can be retrieved with the following command:

export POSTGRES_PASSWORD=$(kubectl get secret --namespace default <SERVICE_NAME>-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)

echo $POSTGRES_PASSWORD
* The instructions are adapted from Bitnami's PostgreSQL Helm Chart.

Test Database Connection The database is accessible within the cluster. This means that when you will have some issues connecting to it via your local environment. You can either connect to a pod that has access to the cluster or connect remotely via Port Forwarding
Connecting Via Port Forwarding
kubectl port-forward --namespace default svc/<SERVICE_NAME>-postgresql 5432:5432 &
    PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U postgres -d postgres -p 5432
Connecting Via a Pod
kubectl exec -it <POD_NAME> bash
PGPASSWORD="<PASSWORD HERE>" psql postgres://postgres@<SERVICE_NAME>:5432/postgres -c <COMMAND_HERE>
Run Seed Files We will need to run the seed files in db/ in order to create the tables and populate them with data.
kubectl port-forward --namespace default svc/<SERVICE_NAME>-postgresql 5432:5432 &
    PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U postgres -d postgres -p 5432 < <FILE_NAME.sql>
2. Running the Analytics Application Locally
In the analytics/ directory:

Install dependencies
pip install -r requirements.txt
Run the application (see below regarding environment variables)
<ENV_VARS> python app.py
There are multiple ways to set environment variables in a command. They can be set per session by running export KEY=VAL in the command line or they can be prepended into your command.

DB_USERNAME
DB_PASSWORD
DB_HOST (defaults to 127.0.0.1)
DB_PORT (defaults to 5432)
DB_NAME (defaults to postgres)
If we set the environment variables by prepending them, it would look like the following:

DB_USERNAME=username_here DB_PASSWORD=password_here python app.py
The benefit here is that it's explicitly set. However, note that the DB_PASSWORD value is now recorded in the session's history in plaintext. There are several ways to work around this including setting environment variables in a file and sourcing them in a terminal session.

Verifying The Application
Generate report for check-ins grouped by dates curl <BASE_URL>/api/reports/daily_usage

Generate report for check-ins grouped by users curl <BASE_URL>/api/reports/user_visits

Project Instructions
Set up a Postgres database with a Helm Chart
Create a Dockerfile for the Python application. Use a base image that is Python-based.
Write a simple build pipeline with AWS CodeBuild to build and push a Docker image into AWS ECR
Create a service and deployment using Kubernetes configuration files to deploy the application
Check AWS CloudWatch for application logs
Deliverables
Dockerfile
Screenshot of AWS CodeBuild pipeline
Screenshot of AWS ECR repository for the application's repository
Screenshot of kubectl get svc
Screenshot of kubectl get pods
Screenshot of kubectl describe svc <DATABASE_SERVICE_NAME>
Screenshot of kubectl describe deployment <SERVICE_NAME>
All Kubernetes config files used for deployment (ie YAML files)
Screenshot of AWS CloudWatch logs for the application
README.md file in your solution that serves as documentation for your user to detail how your deployment process works and how the user can deploy changes. The details should not simply rehash what you have done on a step by step basis. Instead, it should help an experienced software developer understand the technologies and tools in the build and deploy process as well as provide them insight into how they would release new builds.
Stand Out Suggestions
Please provide up to 3 sentences for each suggestion. Additional content in your submission from the standout suggestions do not impact the length of your total submission.

Specify reasonable Memory and CPU allocation in the Kubernetes deployment configuration
In your README, specify what AWS instance type would be best used for the application? Why?
In your README, provide your thoughts on how we can save on costs?
Best Practices
Dockerfile uses an appropriate base image for the application being deployed. Complex commands in the Dockerfile include a comment describing what it is doing.
The Docker images use semantic versioning with three numbers separated by dots, e.g. 1.2.1 and versioning is visible in the screenshot. See Semantic Versioning for more details.