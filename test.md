## Jenkins Setup

[[permission type="modify-agents"]]

### Step 1. Install the Telegraf Agent

This integration uses the HTTP JSON Input Plugin for Telegraf to extract metrics from Jenkins. If you've already installed Telegraf on your server(s), you can skip to Step 2.

[[telegraf]]

### Step 2. Configure Metrics Plugin
Jenkins metrics can be collected using Metrics Plugin of Jenkins.

#### Install Metrics Plugin on your Jenkins server
In the Jenkins web interface, go to Manage Jenkins 

 It provides an API to collect the 
metrics. API can be accessed using an API Key. To generate the API Key, go to Jenkins global configuration
screen ```<jenkins-url>/configure``` and Generate the API Key under the “Metrics” section.

Use HTTP GET reguest URL ```<jenkins-url>/metrics/$KEY/metrics``` to get the metrics.


For details, see the docs [Metrics Plugin](https://wiki.jenkins.io/display/JENKINS/Metrics+Plugin).

### Step 3. Enable the HTTP JSON Input Plugin

Create a file called `jenkins.conf` in `/etc/telegraf/telegraf.d` and enter the following snippet:

   ```
[[inputs.httpjson]]

  ## Prefix to attach to the measurement name
  name_prefix = "jenkins_stats"

  ## URL of each server
  servers = [
    "$JENKINS_URL/metrics/$KEY/metrics"
  ]

  ## HTTP method to use
  method = "GET"

  ## Fields with a field key matching one of the patterns will be discarded
  fielddrop = ["histograms*"]

   ```


### Step 4. Restart Telegraf

Run `sudo service telegraf restart` to restart your Telegraf agent.
