# ephemeral
A wrapper to bonfire to run local repository on ephemeral cluster

### Prerequisits
1. public repository in [quay.io](https://quay.io/)
2. Install Openshift cli ([get oc command here](https://docs.openshift.com/container-platform/4.7/cli_reference/openshift_cli/getting-started-cli.html))

### Setup
1. log into VPN
2. clone [this(ephemeral)](https://github.com/project-koku/ephemeral) repository
3. clone ([koku](https://github.com/project-koku/koku) repository if not already)
4. Get config_yaml.template (see koku administrator)
5. Add ephemeral to your path
     ```
     export PATH=$PATH:~/github/ephemeral
     ```
6. Set the required environment variables. (examples below)
   ```
   export KOKU_HOME=~/github/koku
   export QUAY_REPO=quay.io/testuser/koku
   export AWS_ACCESS_KEY_ID_EPH="[YOUR AWS ACCESS KEY]"
   export AWS_SECRET_ACCESS_KEY_EPH="[YOUR SECRET ACCESS KEY]"
     ```
7. Go to koku project directory
    ```
    cd ${KOKU_HOME}
    pipenv install --dev
    pipenv shell "pre-commit install
    ```
8. install Bonfire
   ```
   pip install crc-bonfire
    ```
9. Get your auth token.
    [HERE](https://oauth-openshift.apps.c-rh-c-eph.8p0c.p1.openshiftapps.com/oauth/token/display)
   1. Save it to the following location(using your favorite editor or just echo it)
    ```
    This command will overwrite(or create) the .eph_token file in your home directory
    echo [Your API Token] > ~/.eph_token 
    ```

### Reserving a namespace
1. log into the ephemeral environment
    ```
   ephemeral login
   ```
2. reserve a namespace (example of 48 hours (defaults to 24 hours if no hours are given)
    ```
   ephemeral reserve 48h
   ```
3. You can check your namespace
    ```
   ephemeral list
   ```
4. You can view the pods (you should not see any resources at this point)
    ```
   ephemeral pods
   ```
   
### Building and Deploying an image
1. build image from local repository
    ```
   ephemeral build-image
   ```
2. deploy image built image
    ```
   ephemeral deploy-image
   ```
3. watch as pods come spin up(at this point you should start seeing koku specific pods, similar to running locally)
    ```
   ephemeral pods
   ```
### Using port forwarding
1. port forward Koku
    ```
   ephemeral port-forward-koku
   ```
2. port forward Masu
    ```
   ephemeral port-forward-masu
   ```
3. port forward services
    ```
   ephemeral port-forward-service
   ```
### Releasing a namespace
1. port forward Koku
    ```
   ephemeral release
   ```