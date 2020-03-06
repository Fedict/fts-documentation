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

The next step will start the docker registry to be able to accept the docker image pushing.

``` BASH
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

In order to use the maven commands, you must install java and maven on the system.

## Moving the git repository

Unzip the file into a directory and preform the needed git commands. These commands will be provided by gitlab itself. After making a project use the instructions giving under the push an existing Git repository. The pipeline is already configured in the file .gitlab-ci.yml.

## Changes to the pipeline

Next step might involve some changes in the .gitlab-ci.yml file, depending on the installation. Changing this file shows again that the current installation and infrastructure isn't a good representation of the reality we're building. For myself, I had to change the docker push command to the following:

``` BASH
docker build -t localhost:5000/sign-validation
```

``` BASH
docker run localhost:5000/sign-validation
```

The last command will start up the application. Now Postman can be used in other to test the sign&validation.

## Documentation

[gitlab documentation](https://docs.gitlab.com/)
