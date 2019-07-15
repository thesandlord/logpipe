# LogPipe
Simple service that will let you pipe logs directly to [Stackdriver Logging](https://cloud.google.com/logging/).

## Background
Google App Engine and Container Engine automatically stream logs to Stackdriver Logging. [Docker](https://www.docker.com) also supports [streaming logs to Stackdriver](https://docs.docker.com/engine/admin/logging/gcplogs/).

However, raw Compute Engine does not. Other cloud VMs also do not. You can install the [logging agent](https://cloud.google.com/logging/docs/agent/installation) on Compute Engine, but that requires you to log to a [common log file, custom log file, or syslog](https://cloud.google.com/logging/docs/view/service/agent-logs).

If you want a simple way to stream logs from the Stdout of any program to Stackdriver Logging, this is for you!

# Install
You can download a pre-compiled binary for your system [here](https://github.com/thesandlord/logpipe/releases).

Otherwise:

    go get -u github.com/thesandlord/logpipe
# Setup

_Note: If you are running on Google Compute Engine, there is no need for any setup._

If you want to use this on your local machine, install the [Google Cloud SDK](cloud.google.com/sdk) and run:


    gcloud auth application-default login

If you are running on a VM outside Google Cloud, follow the steps [here](https://developers.google.com/identity/protocols/application-default-credentials#howtheywork) to set the `GOOGLE_APPLICATION_CREDENTIALS` environment variable.

# Usage

```
Usage:
  logpipe [OPTIONS]

Application Options:
  -p, --project= Google Cloud Platform Project ID
  -l, --logname= The name of the log to write to (default: default)

Help Options:
  -h, --help     Show this help message
```

## Examples

This will log all the output from a Node.js program

    node app.js | logpipe -p <YOUR_PROJECT_ID>

This will log the word "hello" 5 times to the "default" log

    yes hello | head -n 5 | logpipe -p <YOUR_PROJECT_ID>

This will log the word "test" to the "tester" log

    echo test | logpipe -p <YOUR_PROJECT_ID> -l tester
