Jenkins Pipeline Summary - Jenkins running Master and slave configuartion
Agent
Runs on Jenkins node labeled dev with required tools installed.

Environment
Sets Java, Maven paths, and Tomcat webapps directory for deployment.

Checkout
Clones the repo and checks out the dev-stage branch.

Build WAR
Runs Maven clean package in busservices to build busservices.war.

Move WAR
Creates /opt/stage/war and moves the WAR there for deployment.

Deploy WAR to Tomcats
Stops Tomcat, cleans old deployments, copies new WAR to webapps, then restarts Tomcat servers.

Optional Wait & Log Check
Waits 30 seconds, then tails last 60 lines of Tomcat logs for verification.

Post Actions
Echoes success or failure message after pipeline completion.
