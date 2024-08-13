# Building a Java Application with Maven Using Jenkins

This guide demonstrates how to use Jenkins to build a Java application with Maven. For this tutorial, we are using the built-in Jenkins node to execute the jobs. In future configurations, we will explore using Jenkins agents for handling build jobs. Additionally, we will use a Freestyle project for this setup.

The tutorial covers the following tasks:

1. Create a Jenkins job to build and test a Java application using Maven.
2. Run the Jenkins job to build and test the Java application.
3. View the build results in Jenkins.
4. Configure the job to deploy the JAR file locally.
5. Visualize the build results graphically.

The example Java application used is from the GitHub repository [simple-java-maven-app](https://github.com/jenkins-docs/simple-java-maven-app/tree/master). It outputs "Hello world!" and includes unit tests. The test results are saved in a **JUnit XML report**, which will be used for visualization.

## Prerequisites

1. Ensure your Jenkins server is up and running.

## Step-by-Step Guide

### Step 1: Install Necessary Plugins and Tools

1. **Install the Maven Integration Plugin:**
   - Go to **Manage Jenkins** -> **Manage Plugins**.
   - In the **Available** tab, search for `Maven Integration (Build Tools)`.
   - Install this plugin.

   ![Install Maven Integration Plugin](image-4.png)

2. **Configure Maven Installation:**
   - Go to **Manage Jenkins** -> **Global Tool Configuration**.
   - Scroll down to the **Maven** section.
   - Add a new Maven installation:
     - Give it a name, e.g., `Maven-jenkins`.
     - Select the required version and save.

   ![Configure Maven Installation](image-5.png)

### Step 2: Create a Jenkins Job

1. Log in to your Jenkins server and click on **New Item**.
2. Enter a name for your job, e.g., "Simple Java Maven App", and select **Freestyle project**.
3. Click **OK** to create the job.

   ![Create New Job](image.png)

4. In the **Source Code Management** section, select **Git** and enter the URL of the GitHub repository:

   ```sh
   https://github.com/jenkins-docs/simple-java-maven-app.git
   ```

   ![Configure Source Code Management](image-1.png)

5. Save the configuration and build the job. This will clone the repository from GitHub.

   ![Build Job](image-2.png)

6. In the workspace section, you should see the repository files.

   ![Workspace Files](image-3.png)

### Step 3: Configure the Build Step

1. Go to the **Configure** section of the job.
2. In the **Build** section, click **Add build step** and select **Invoke top-level Maven targets**.

   ![Add Build Step](image-6.png)

3. Select your `Maven Version`.

4. In the **Goals** field, enter the following command to clean, package, and skip tests:

   ```sh
   -B -DskipTests clean package
   ```

   ![Configure Maven Goals](image-7.png)

5. Save the configuration and build the job. This will build and package the application. Check the console output for a successful build.

   ![Build Output](image-8.png)

### Step 4: Configure the Test Step

1. Go to the **Configure** section of the job.
2. In the **Build** section, click **Add build step** and select **Invoke top-level Maven targets**.
3. Select your `Maven Version`.
4. In the **Goals** field, enter the following command to run tests:

   ```sh
   test
   ```

   ![Configure Test Goals](image-9.png)

5. Save the configuration and build the job. This will run the tests on the application. Check the console output for a successful test run.

   ![Test Output](image-10.png)

### Step 5: Deploy the JAR File

1. Go to the **Configure** section of the job.
2. In the **Build** section, click **Add build step** and select **Execute shell**.
3. In the **Command** field, enter the command to run the JAR file:

   ```sh
   java -jar <path_to_your_jar_file>
   ```

   Replace `<path_to_your_jar_file>` with the path to your JAR file, typically found in `/var/jenkins_home/workspace/your_job_name/target/`.

   ![Deploy JAR](image-12.png)

4. Save the configuration and build the job. This will run the Java application. Check the console output for a successful run.

   ![Deployment Output](image-13.png)

### Step 6: Visualize the Build Results

1. In the **Workspace** directory of the job, navigate to:

   **Workspace** -> **YOUR_JOB_NAME** -> **target** -> **surefire-reports**. Locate the XML file containing the build information.

   ![Locate XML Report](image-14.png)

2. Return to the job configuration and scroll down to **Post-build Actions**. Click **Add post-build action** and select **Publish JUnit test result report**.

   ![Add Post-build Action](image-15.png)

3. In the **Test report XMLs** field, enter the path to the XML files:

   ```sh
   target/surefire-reports/*.xml
   ```

   ![Configure Test Result Report](image-16.png)

4. Save the configuration and build the job. This will publish the test results on the Jenkins dashboard with graphical representations such as graphs and charts.

   ![Test Result Visualization](image-17.png)

---

## Conclusion

You have successfully set up Jenkins to build, test, deploy, and visualize the results of a Java application using Maven.