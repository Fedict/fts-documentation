# Install guide

This guide will explain how to setup the project so that the gitlab pipeline and an application can be tested. These steps can serve as a helpful guide when deploying the application on a server, but different environment require different settings and configuration.

This guide merely serves as a way to confirm everything works as intended. And enable a user or other interested party to confirm the features and work-flow of the application.

## The gitlab instance

At the basis of this guide will be a gitlab instance. This will be both the location of the pipeline and the repository.

On a side note, the current main difference with a server based setup is that on the server setup, gitlab would also serve as a package repository for the docker images. Meaning that the actual code, the pipelines and the docker images would be hosted, run and created on the same instance. This allows for an easier setup in more complex environments such as high security environments tend to be.

Install the gitlab instance following the guide on the gitlab documentation page. This will help you during the installation process with configuring the gitlab instance. During my installation I had the issue that the https certificate could not be validated. Changing the url to http solved this. As this is for a demo project, the reduced security matters less. So instead of the https version of the command, use the http version.

If the lower security is a hindrance , do note that the url can always be changed after the installation. This by editing the `/etc/gitlab/gitlab.rb` file and setting the url back to what is required for your purpose.

In the following command replace the IP address with the local IP address of your machine.

``` BASH
sudo EXTERNAL_URL="http://192.168.1.247" apt-get install gitlab-ce
```

If this command return that it cannot find the application, run the following command to add the gitlab package. This is needed because gitlab is note native to the ubuntu repositories. Rerun the command after adding the package.

``` BASH
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
```

After the install, you can start the gitlab instance with the following command. While this command does not in essence start gitlab, it is required to load the configuration needed. In the future another command can be used in order to start the instance `sudo gitlab-ctl start`. First time setup requires the instance to be configured before first running.

``` BASH
sudo gitlab-ctl reconfigure
```

### The gitlab instance runner

Next we must add the gitlab-runner repositories to the package manager in Ubuntu. This is due to the same reason gitlab itself had to be added. And since not every gitlab instance requires a gitlab runner, this packages are separated.

``` BASH
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
```

When this step is completed the installation can start.

``` BASH
sudo apt-get install gitlab-runner
```

Next we should register the gitlab runner. First navigate to the settings of CI/CD, under the header runner you should find a token. Copy paste the token as this will be needed later on. With the command you can start the registration of the runner. Follow the instructions during the install. When asked about the type of runner select `shell`.

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

In the gitlab pipeline configuration maven commands are used. These require commands require maven and java to be installed and configured.

## Moving the git repository

Unzip the file into a directory and preform the needed git commands. These commands will be provided by gitlab itself. After making a project use the instructions giving under the push an existing Git repository. The pipeline is already configured in the file .gitlab-ci.yml.

## Changes to the pipeline

Next step might involve some changes in the .gitlab-ci.yml file, depending on the installation. Changing this file shows again that the current installation and infrastructure isn't a good representation of the reality we're building. For myself, I had to change the `docker push` command to the following:

``` BASH
docker build -t localhost:5000/sign-validation
```

The last command will start up the application. Now Postman can be used in other to test the sign&validation.

``` BASH
docker run localhost:5000/sign-validation
```

## Documentation

[gitlab documentation](https://docs.gitlab.com/)
