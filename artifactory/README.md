Docker Installation
Create your Artifactory home directory and an empty system.yaml file. The user creating the folder should be the user running the docker run.

* The following steps assume that `$JFROG_HOME` environment variable is created in the system. For the correct location of `$JFROG_HOME`, see to System Directories - JFrog Product Directory Structure

    ```sh
    mkdir -p $JFROG_HOME/artifactory/var/etc/
    cd $JFROG_HOME/artifactory/var/etc/
    touch ./system.yaml
    chown -R $UID:$GID $JFROG_HOME/artifactory/var
    ```

* Run the following command in addition if you are using Docker on a Mac/Linux machine.

    ```sh
    chmod -R 777 $JFROG_HOME/artifactory/var
    ```

* Customize the product configuration (optional) including database, Java Opts, and filestore.

    For Docker installations, verify that the host's ID shared.node.id and IP shared.node.ip are added to the system.yaml.
    If these are not manually added, they are automatically resolved as the the container's IP, meaning other nodes and services will not be able to reach this instance.

* Start the Artifactory container using the process that is relevant for your system.

    ```sh
    docker run --name artifactory -v $JFROG_HOME/artifactory/var/:/var/opt/jfrog/artifactory -d -p 8081:8081 -p 8082:8082 releases-docker.jfrog.io/jfrog/artifactory-cpp-ce:latest
    ```

    > The Docker run command exposes more than one port: 8081 for Artifactory REST APIs and 8082 for all other uses.

* Manage Artifactory using native Docker commands.

    ```sh
    # Examples
    docker ps
    docker stop artifactory
    ```

* Access Artifactory from your browser at: http://SERVER_HOSTNAME:8082/ui/. For example, on your local machine: http://localhost:8082/ui/.
Check the Artifactory log.

    ```sh
    docker logs -f artifactory
    Configuring the Log Rotation of the Console Log
    ```

    The console.log file can grow quickly since all services write to it. Learn more on how to configure log rotation.

* In an effort to provide a more secure Artifactory image, Artifactory now uses the Redhat UBI Micro base image. Some of the tools that were available in the Artifactory image are not available in this more secure image. For more information, see JFrog Products Container Base Image.
