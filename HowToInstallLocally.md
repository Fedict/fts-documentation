# Install guide

This guide will explain how to install everything in the project in a few easy commands. Some steps are automated, while others are done manual.

## The gitlab instance

Install the gitlab instance following the guide on the gitlab documentation page. This will guide you during the installation process and help with configuring the gitlab instance. During my installation I had the issue that the https certificate could not be validated. Changing the url to http solved this. As this is for a demo project, the reduced security matters less. So instead of the https version of the command. use the http version.

``` BASH
sudo EXTERNAL_URL="https://gitlab.example.com" apt-get install gitlab-ce
```

I did the following:

``` BASH
sudo EXTERNAL_URL="http://192.168.1.247" apt-get install gitlab-ce
```

If this command return that it cannot find the application, run the following command to add the gitlab package.

``` BASH
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
```

After the install, you can start the gitlab instance with the following command.

``` BASH
sudo gitlab-ctl reconfigure
```

### The gitlab instance runner

First we must add the gitlab-runner repositories to the package manager in linux

``` BASH
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
```

When this step is completed the install can start.

``` BASH
sudo apt-get install gitlab-runner
```

Next we should register the gitlab runner. First navigate to the settings of CI/CD, under the header runner you should find a token. Copy paste the token as this will be needed later on. Enter the needed command and follow the steps as provided.

``` BASH
sudo gitlab-runner register
```

After installing docker on the machine, the gitlab runner might not have access to the docker service. This can be resolved by doing to following:

``` BASH
sudo usermod -aG docker gitlab-runner
sudo service docker restart
```

Next step would be to have access to the docker registry on the local machine. Using the command to install the registry from docker itself is just the first part of the installation. When running the command now, the response would be access denied. The runner needs to be able to log onto the docker registry in order to be able to push anything.
This can be done by changing the configuration in the gitlab.yml file.

``` BASH
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

The nano command enables opens a linux based writting program, much like vim. Here you'd be able to add the correct configuration for your machine.

``` BASH
sudo nano /etc/gitlab/gitlab.rb
```

``` BASH
registry:
  enabled: true
  host: registry.192.168.1.247
  port: 5005
  api_url: http://192.168.1.247:5000/
  key: config/registry.key
  path: shared/registry
  issuer: gitlab-issuer
```

## Moving the git repository

Unzip the file into a directory and preform the needed git commands. These commands will be provided by gitlab itself. After making a project use the instructions giving under the push an existing Git repository. The pipeline is already configured in the file .gitlab-ci.yml.

[gitlab documentation](https://docs.gitlab.com/)
