cloudfoundry-sticky-session
===========================

Simple Java web app that prints Cloud Foundry environment variables and cookies and is useful to showcase sticky sessions.

It also lets you test the HealthManager by sending a "http://host/?shutdown=true" request. The shutdown command will kill the App Instance and PAS' Health Management will start a new instance of the App automatically for you.

How to use
==========

1. Clone this repo locally. In this example, I will be using a `/work` directory:

```
cd /work
git clone https://github.com/rm511130/cloudfoundry-sticky-session
cd /work/cloudfoundry-sticky-session
```

2. Use Maven to build a WAR file which will be placed under the `target` directory

```
mvn clean package
```

Note: you may see some warnings, but you can safely continue to step 3 if you don't see errors during the build process.

3. Make sure you're targeting a valida PAS environment. For example:

```
cf t
api endpoint:   https://api.system.pcf4u.com
api version:    2.131.0
user:           rmeira@pivotal.io
org:            demo
space:          demo
```

If you don't get a similar response, it probably means that you have to first use the `cf api` and `cf login` commands to point and log into the correct PAS environment.

4. Time to push your App:

```
cf push
```

You should see an output similar to the one shown below:

```
Pushing from manifest to org demo / space demo as rmeira@pivotal.io...
Using manifest file /work/cloudfoundry-sticky-session/manifest.yml
Getting app info...
Creating app with these attributes...
+ name:        sticky-session
  path:        /work/cloudfoundry-sticky-session/target/StickySessionApp-1.0.war
+ instances:   4
+ memory:      640M
  routes:
+   sticky-session.apps.pcf4u.com

Creating app sticky-session...
Mapping routes...
Comparing local files to remote cache...
Packaging files to upload...
Uploading files...
 5.11 KiB / 5.11 KiB [=====================================================================================================] 100.00% 1s

Waiting for API to complete processing files...

Staging app and tracing logs...
   Downloading r_buildpack...
   Downloading go_buildpack...
   Downloading ruby_buildpack...
   Downloading nodejs_buildpack...
   Downloading dotnet_core_buildpack...
   Downloaded go_buildpack
   Downloaded nodejs_buildpack
   Downloaded r_buildpack
   Downloading staticfile_buildpack...
   Downloading python_buildpack...
   Downloading php_buildpack...
   Downloaded dotnet_core_buildpack
   Downloaded ruby_buildpack
   Downloading binary_buildpack...
   Downloading cppcms-buildpack...
   Downloaded cppcms-buildpack
   Downloaded python_buildpack
   Downloading java_buildpack_offline...
   Downloaded php_buildpack
   Downloading nginx_buildpack...
   Downloaded staticfile_buildpack
   Downloaded binary_buildpack
   Downloaded nginx_buildpack
   Downloaded java_buildpack_offline
   Cell 64006a29-a421-4ec0-9e77-8fdb376a8c1c creating container for instance 1de7192f-0332-4f60-b925-6228df1f1ca7
   Cell 64006a29-a421-4ec0-9e77-8fdb376a8c1c successfully created container for instance 1de7192f-0332-4f60-b925-6228df1f1ca7
   Downloading app package...
   Downloaded app package (5.1K)
   -----> Java Buildpack v4.18 (offline) | https://github.com/cloudfoundry/java-buildpack.git#a0df7d0
   -----> Downloading Jvmkill Agent 1.16.0_RELEASE from https://java-buildpack.cloudfoundry.org/jvmkill/bionic/x86_64/jvmkill-1.16.0_RELEASE.so (found in cache)
   -----> Downloading Open Jdk JRE 1.8.0_202 from https://java-buildpack.cloudfoundry.org/openjdk/bionic/x86_64/openjdk-1.8.0_202.tar.gz (found in cache)
          Expanding Open Jdk JRE to .java-buildpack/open_jdk_jre (2.1s)
          JVM DNS caching disabled in lieu of BOSH DNS caching
   -----> Downloading Open JDK Like Memory Calculator 3.13.0_RELEASE from https://java-buildpack.cloudfoundry.org/memory-calculator/bionic/x86_64/memory-calculator-3.13.0_RELEASE.tar.gz (found in cache)
          Loaded Classes: 9755, Threads: 250
   -----> Downloading Client Certificate Mapper 1.8.0_RELEASE from https://java-buildpack.cloudfoundry.org/client-certificate-mapper/client-certificate-mapper-1.8.0_RELEASE.jar (found in cache)
   -----> Downloading Container Security Provider 1.16.0_RELEASE from https://java-buildpack.cloudfoundry.org/container-security-provider/container-security-provider-1.16.0_RELEASE.jar (found in cache)
   -----> Downloading Tomcat Instance 9.0.16 from https://java-buildpack.cloudfoundry.org/tomcat/tomcat-9.0.16.tar.gz (found in cache)
          Expanding Tomcat Instance to .java-buildpack/tomcat (0.3s)
   -----> Downloading Tomcat Access Logging Support 3.3.0_RELEASE from https://java-buildpack.cloudfoundry.org/tomcat-access-logging-support/tomcat-access-logging-support-3.3.0_RELEASE.jar (found in cache)
   -----> Downloading Tomcat Lifecycle Support 3.3.0_RELEASE from https://java-buildpack.cloudfoundry.org/tomcat-lifecycle-support/tomcat-lifecycle-support-3.3.0_RELEASE.jar (found in cache)
   -----> Downloading Tomcat Logging Support 3.3.0_RELEASE from https://java-buildpack.cloudfoundry.org/tomcat-logging-support/tomcat-logging-support-3.3.0_RELEASE.jar (found in cache)
   Exit status 0
   Uploading droplet, build artifacts cache...
   Uploading droplet...
   Uploading build artifacts cache...
   Uploaded build artifacts cache (131B)
   Uploaded droplet (54.5M)
   Uploading complete
   Cell 64006a29-a421-4ec0-9e77-8fdb376a8c1c stopping instance 1de7192f-0332-4f60-b925-6228df1f1ca7
   Cell 64006a29-a421-4ec0-9e77-8fdb376a8c1c destroying container for instance 1de7192f-0332-4f60-b925-6228df1f1ca7

Waiting for app to start...

name:              sticky-session
requested state:   started
routes:            sticky-session.apps.pcf4u.com
last uploaded:     Mon 17 Jun 21:59:22 EDT 2019
stack:             cflinuxfs3
buildpacks:        client-certificate-mapper=1.8.0_RELEASE container-security-provider=1.16.0_RELEASE
                   java-buildpack=v4.18-offline-https://github.com/cloudfoundry/java-buildpack.git#a0df7d0 java-opts
                   java-security jvmkill-agent=1.16.0_RELEASE open-jdk-like-jre=1...

type:            web
instances:       1/4
memory usage:    640M
start command:   JAVA_OPTS="-agentpath:$PWD/.java-buildpack/open_jdk_jre/bin/jvmkill-1.16.0_RELEASE=printHeapHistogram=1
                 -Djava.io.tmpdir=$TMPDIR -XX:ActiveProcessorCount=$(nproc)
                 -Djava.ext.dirs=$PWD/.java-buildpack/container_security_provider:$PWD/.java-buildpack/open_jdk_jre/lib/ext
                 -Djava.security.properties=$PWD/.java-buildpack/java_security/java.security $JAVA_OPTS -Daccess.logging.enabled=false
                 -Dhttp.port=$PORT" &&
                 CALCULATED_MEMORY=$($PWD/.java-buildpack/open_jdk_jre/bin/java-buildpack-memory-calculator-3.13.0_RELEASE
                 -totMemory=$MEMORY_LIMIT -loadedClasses=11092 -poolType=metaspace -stackThreads=250 -vmOptions="$JAVA_OPTS") && echo
                 JVM Memory Configuration: $CALCULATED_MEMORY && JAVA_OPTS="$JAVA_OPTS $CALCULATED_MEMORY" && MALLOC_ARENA_MAX=2
                 JAVA_OPTS=$JAVA_OPTS JAVA_HOME=$PWD/.java-buildpack/open_jdk_jre exec $PWD/.java-buildpack/tomcat/bin/catalina.sh run
     state      since                  cpu    memory          disk       details
#0   starting   2019-06-18T01:59:28Z   0.0%   41.9K of 640M   8K of 1G   
#1   starting   2019-06-18T01:59:28Z   0.0%   0 of 640M       0 of 1G    
#2   running    2019-06-18T01:59:39Z   0.0%   41.9K of 640M   8K of 1G   
#3   starting   2019-06-18T01:59:28Z   0.0%   38.1K of 640M   8K of 1G 
```

5. Once the App is running, use a browser to access its URL. You can find the URL (or route) using the command shown below:

```
cf app sticky-session | grep route
routes:            sticky-session.apps.pcf4u.com
```

When you point your browser to `http://sticky-session.apps.pcf4u.com` you should see a page on your browser with a content similar to the example shown below:

```
kill this app instance

Sticky session is NOT enabled. Refreshing the browser should route to random app instances.

start a sticky session

VCAP_APPLICATION env var: {"application_id":"860c9d22-5a44-4dff-9e91-24760e7f1565","application_name":"sticky-session","application_uris":["sticky-session.apps.pcf4u.com"],"application_version":"18cf56e1-f0c8-4279-aaa8-5b83154e6507","cf_api":"https://api.system.pcf4u.com","host":"0.0.0.0","instance_id":"70f44316-d0d7-4210-483a-0ee4","instance_index":2,"limits":{"disk":1024,"fds":16384,"mem":640},"name":"sticky-session","port":8080,"space_id":"029d4391-16e8-4681-8b9b-58c72ad0081b","space_name":"demo","uris":["sticky-session.apps.pcf4u.com"],"version":"18cf56e1-f0c8-4279-aaa8-5b83154e6507"}

Port: 8080
```

6. Paying attention to the `instance_index:` value, go ahead and hit the `refresh` button on your browser.
You should see that the index # changes between 0 and 3 - corresponding to the 4 App Instances you pushed.

7. Now click on the `start a sticky session` link and then `refresh` your browser. You should see an output similar to the one shown below:

```
kill this app instance

Cookies: 
Name: JSESSIONID
Value: F195900C5CCB7E05F7DA2DF1EA9DD613
Name: __VCAP_ID__
Value: 82562706-72a1-437a-6b66-183c

Sticky session is enabled. Refreshing the browser should keep routing to the same app instance.

VCAP_APPLICATION env var: {"application_id":"860c9d22-5a44-4dff-9e91-24760e7f1565","application_name":"sticky-session","application_uris":["sticky-session.apps.pcf4u.com"],"application_version":"18cf56e1-f0c8-4279-aaa8-5b83154e6507","cf_api":"https://api.system.pcf4u.com","host":"0.0.0.0","instance_id":"82562706-72a1-437a-6b66-183c","instance_index":0,"limits":{"disk":1024,"fds":16384,"mem":640},"name":"sticky-session","port":8080,"space_id":"029d4391-16e8-4681-8b9b-58c72ad0081b","space_name":"demo","uris":["sticky-session.apps.pcf4u.com"],"version":"18cf56e1-f0c8-4279-aaa8-5b83154e6507"}

Port: 8080
```

8. Hit the `refresh` button on your browser and notice that the `instance_index` number does not change. In the code you git-cloned, you will find `./src/main/java/com/iamjambay/cloudfoundry/stickysession/MainServlet.java`. Take a look at the code to see that when your clicked on `start a sticky session` you effectively switched to `sticky=true` and from there on PAS understood that you wished to maintain a sticky-session with the specific App Instance your browser was connected to.

```java
                if ("true".equals(request.getParameter("sticky"))) {
                        request.getSession( true );
                        sticky = true;
                }
```


9. Using the `kill this app instance` link will produce the following results:
   a. The current App Instance will be killed
   b. Your request will be handled by a different App Instance. Your App will stick to this new App Index #.
   c. PAS will start a new App Instance to compensate for the one you killed.
   
   
10. To start over:
   a. Use your browser's development tools to delete the JSESSION cookie
   b. Pass `/?sticky=false` to your App
   
   
