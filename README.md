# sfdx-gitlab-org

For a fully guided walk through of setting up and configuring this sample, see the [Continuous Integration Using Salesforce DX](https://trailhead.salesforce.com/modules/sfdx_travis_ci) Trailhead module.

This repository shows one way you can successfully setup Salesforce DX with GitLab CI. We make a few assumptions in this README:

- You know how to get your GitLab repository setup with GitLab CI. (Here's their [Getting Started guide](https://docs.gitlab.com/ee/ci/README.html).)
- You have properly setup JWT-Based Authorization Flow (i.e. headless). We recommended using [these steps for generating your Self-Signed SSL Certificate](https://devcenter.heroku.com/articles/ssl-certificate-self). 

If any any of these assumptions aren't true, the following steps won't work.

## Getting Started

1) Make sure you have the Salesforce CLI installed. Check by running `sfdx force --help` and confirm you see the command output. If you don't have it installed you can download and install it from [here](https://developer.salesforce.com/tools/sfdxcli).

2) Confirm you can perform a JWT-based auth: `sfdx force:auth:jwt:grant --clientid <your_consumer_key> --jwtkeyfile server.key --username <your_username> --setdefaultdevhubusername`

   **Note:** For more info on setting up JWT-based auth see [Authorize an Org Using the JWT-Based Flow](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_auth_jwt_flow.htm) in the [Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev).

3) [Mirror](https://docs.gitlab.com/ee/workflow/repository_mirroring.html) this repo into your GitLab account.

4) Clone your mirrored repo locally: `git clone https://gitlab.com/<gitlab_username>/sfdx-gitlab-org.git`

5) From your JWT-Based connected app on Salesforce, retrieve the generated `Consumer Key`.

6) Setup GitLab CI / CD [environment variables](https://gitlab.com/help/ci/variables/README#variables) for your Salesforce `Consumer Key` and `Username`. Note that this username is the username that you use to access your Salesforce org.

    Create an environment variable named `SF_CONSUMER_KEY` and set it as protected.

    Create an environment variable named `SF_USERNAME` and set it as protected.

7) Encrypt your `server.key` file that you generated previously and add the encrypted file (`server.key.enc`) to the folder named `assets`.

    `openssl aes-256-cbc -salt -e -in server.key -out server.key.enc -k password`

6) Setup GitLab CI / CD [environment variable](https://gitlab.com/help/ci/variables/README#variables) for the password you used to encrypt your `server.key` file.

    Create an environment variable named `SERVER_KEY_PASSWORD` and set it as protected.

9) IMPORTANT! Don't check in your `server.key` file to GitLab. You should never store keys or certificates in a public place.
 
And you should be ready to go! Now when you commit and push a change, your change will kick off a GitLab CI build.

Enjoy!

## Contributing to the Repository ###

If you find any issues or opportunities for improving this repository, fix them!  Feel free to contribute to this project by [forking](http://help.github.com/fork-a-repo/) this repository and make changes to the content.  Once you've made your changes, share them back with the community by sending a pull request. Please see [How to send pull requests](http://help.github.com/send-pull-requests/) for more information about contributing to Github projects.

## Reporting Issues ###

If you find any issues with this demo that you can't fix, feel free to report them in the [issues](https://github.com/forcedotcom/sfdx-gitlab-org/issues) section of this repository.