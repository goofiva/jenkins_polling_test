Set up Jenkins Github pull request builder
---

- **Set up GitHub API Personal Access tokens.**
  - Create or use an existing Github account that will be use to access repo. For this example we'll use: "my_company_jenkins."
  - Goto https://github.com/settings/tokens > Click Button: __Generate new token.__
  
        Token description: my_company_jenkins

          ✓ repo
          ✓ repo:status
          ✓ repo_deployment
          ✓ public_repo
          ✓ repo:invite

  - Copy the __Github API token__ somewhere, we'll be needing this later.


- **Install Jenkins Plugin.**
  - Goto Jenkins > Manage Jenkins > Manage Plugins > Select the __Available__ tab.
  - Find and install __GitHub Pull Request Builder__.
    (NOTE: Ignore the security message, the problem has been addressed after versions 1.4.0)


- **Set up GitHub Pull Request Builder Plugin.**
  - Goto Jenkins > Manage Jenkins > Configure System > GitHub Pull Request Builder > Click Button: __Add__
    - GitHub Pull Request Builder

        - GitHub Auth

              GitHub Server API URL: https://api.github.com
              Shared secret: my_company_jenkins's github api token
              Credentials: access tokens
              Description: my_company_jenkins

      - Application Setup
        - Commit Status Build Result

              - Build Result: SUCCESS
                Message: It worked!

              - Build Result: ERROR
                Message: It errored!

              - Build Result: FAILURE
                Message: It failed!

  - Goto Jenkins > Manage Jenkins > Configure System > GitHub Pull Request Builder > Click Button: __Advanced...__

        Crontab line: \*/5 \* \* \* \*


- **Configure the jenkins job.**
  - Goto Jenkins > JENKINS-PR-JOB-NAME > Configure
    - General
      - ✓ Check GitHub project.

            Project url: https://github.com/__username__/__repo__

    - Source Code Management:
      - ✓ Select __Git__
        - Repositories

              Repository URL: https://github.com/__username__/__repo__.git
              Credentials: __your-cred__

          - Button: __Advanced...__

                Name: origin
                Refspec: +refs/pull/${ghprbPullId}/*:refs/remotes/origin/pr/${ghprbPullId}/*

        - Branches to build

              Branch Specifier: ${sha1}

    - Build Triggers
      - ✓ Check __GitHub Pull Request Builder__

            GitHub API credentials: https://api.github.com: my_company_jenkins
            Admin list: my_company_jenkins

        - Click Button: __Advanced...__

              Crontab line: \*/5 \* \* \* \*
