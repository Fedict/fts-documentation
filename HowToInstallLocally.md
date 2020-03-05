# Install guide

This guide will explain how to install everything in the project in a few easy commands. Some steps are automated, while others are done manual.

## The gitlab instance

Install the gitlab instance following the guide on the gitlab documentation page. This will guide you during the installation process and help with configuring the gitlab instance. During my installation I had the issue that the https certificate could not be validated. Changing the url to http solved this. As this is for a demo project, the reduced security matters less. So instead of the https version of the command. use the http version.

``` BASH
sudo EXTERNAL_URL="https://gitlab.example.com" apt-get install gitlab-ee
```

I did the following:

``` BASH
sudo EXTERNAL_URL="http://gitlab.example.com" apt-get install gitlab-ee
```

After the install, you can start the gitlab instance with the following command.

``` BASH
sudo gitlab-ctl reconfigure
```

### The gitlab instance runner

[gitlab documentation](https://docs.gitlab.com/)
