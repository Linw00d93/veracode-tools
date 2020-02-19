# Veracode Tools Docker Image (CentOS Base Image)

Docker image with all Veracode tools pre-installed. This is not an official Veracode project, Veracode support will not be able to provide assistance with issues.

    docker pull ctcampbellcom/veracode-tools:centos

For all the commands below you can bind mount your application source/build directory into `/workspace`, which is the working directory when running commands interactively.

    docker run -it -v /source-dir:/workspace ...

## Java API Wrapper

    docker run -it -v /source-dir:/workspace \
    --rm ctcampbellcom/veracode-tools java -jar /veracode/veracode-wrapper.jar

## Python/HTTPie

### Option 1: Bind mount API credentials

Set up a local `~/.veracode/credentials` file with API credentials:

    [default]
    veracode_api_key_id = <YOUR_API_KEY_ID>
    veracode_api_key_secret = <YOUR_API_KEY_SECRET>

then bind mount into the container:

    docker run -it -v /source-dir:/workspace -v ~/.veracode:/root/.veracode \
    --rm ctcampbellcom/veracode-tools:centos http -A veracode_hmac https://api.veracode.com/appsec/v1/applications

### Option 2: Provide environment variables

    docker run -it -v /source-dir:/workspace --env VERACODE_API_KEY_ID=shf389f3j... --env VERACODE_API_KEY_SECRET=sijfsnfsn... \
    --rm ctcampbellcom/veracode-tools:centos http -A veracode_hmac https://api.veracode.com/appsec/v1/applications

## SourceClear

You may need to install an appropriate build system for SourceClear.

## Option 1: Bind mount an `agent.yml` file

    docker run -it -v /source-dir:/workspace -v ~/.srcclr/agent.yml:/root/.srcclr/agent.yml \
    --rm ctcampbellcom/veracode-tools:centos srcclr scan

## Option 2: Provide an environment variable

    docker run -it -v /source-dir:/workspace --env SRCCLR_API_TOKEN=eyJhbGciOi... \
    --rm ctcampbellcom/veracode-tools:centos srcclr scan
