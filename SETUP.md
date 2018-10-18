Set up Jenkins Github pull request builder
---

- **Set up GitHub API Personal Access tokens.**
  - Create or use an existing Github account that will be use to access repo. For example: my_company_jenkins.
  - Goto https://github.com/settings/tokens > Button: Generate new token.
    - Token description: my_company_jenkins
      ✓ repo
        ✓ repo:status
        ✓ repo_deployment
        ✓ public_repo
        ✓ repo:invite 


- **Install Jenkins Plugin.**
  - Goto Manage Jenkins > Manage Plugins > Available > GitHub Pull Request Builder.
    (NOTE: Ignore the security message, the problem has been addressed after versions 1.4.0)
  
  
- **Set up GitHub Pull Request Builder Plugin.**
  - Goto Jenkins > Manage Jenkins > Configure System > GitHub Pull Request Builder > Button: Add
    - GitHub Pull Request Builder
        - GitHub Auth
          GitHub Server API URL: https://api.github.com
          Shared secret: my_company_jenkins's github api token
          Credentials: access token
          Description: my_company_jenkins
      - Application Setup
        - Commit Status Build Result
          - Build Result: SUCCESS
            Message: It worked!
          - Build Result: ERROR
            Message: It errored!
          - Build Result: FAILURE
            Message: It failed!
  - Goto Jenkins > Manage Jenkins > Configure System > GitHub Pull Request Builder > Button: Advanced...
    - Crontab line: \*/5 \* \* \* \* ( check for pull requests every 5 minutes )


- **Configure the jenkins job.**
  - Goto Jenkins > JENKINS-PR-JOB-NAME > Configure
    - General
      - ✓ GitHub project.
        - Project url: https://github.com/__username__/__repo__
    - Source Code Management:
      - ✓ Git
        - Repositories
          - Repository URL: https://github.com/__username__/__repo__.git
          - Credentials: __your-cred__
          - Button: Advanced...
            - Name: origin
            - Refspec: +refs/pull/${ghprbPullId}/*:refs/remotes/origin/pr/${ghprbPullId}/*
        - Branches to build
          - Branch Specifier: ${sha1}
    - Build Triggers
      - ✓ GitHub Pull Request Builder
        GitHub API credentials: https://api.github.com: my_company_jenkins
        Admin list: $github-username
        - Advanced...
          Crontab line: \*/5 \* \* \* \*
