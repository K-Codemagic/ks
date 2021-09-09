##Build Triggers using codemagic.yaml:

A Build trigger automatically starts a build whenever you make any changes to your source code. You can configure the trigger to build your code on any changes to the source repository or only changes that match certain criteria.

Triggering Via Workflow Editor: https://docs.codemagic.io/flutter-configuration/automatic-build-triggering/
Triggering via codemagic.yaml: https://docs.codemagic.io/yaml/yaml-getting-started/#triggering
Setting up Webhook: https://docs.codemagic.io/flutter-configuration/webhooks/

####Some useful terms to understand Triggering:

Event: Select the repository event to invoke your trigger.

Push: Set your trigger to start a build on commits to a particular branch.

Push new tag: Set your trigger to start a build on commits that contain a particular tag. Codemagic will automatically build the tagged commit whenever you create a tag for this app. 

Pull request: Set your trigger to start a build on commits to a pull request. You have to specify whether the watched branch is the source or the target of the pull request.

Some Examples for Building Triggers:

    triggering:
      events:
        - push
        - pull_request
        - tag
      branch_patterns:
        - pattern: 'develop*'             #pattern
          include: true                   #include is for including or excluding the pattern
          source: true                    #source true/false is for pull request triggers to understand if pattern is for source or target branch
        - pattern: 'feature*'
          include: false
          source: true
      tag_patterns:
        - pattern: '*'
          include: true
 
####Conditional build triggers:

You can avoid unnecessary builds when functional components of your repository were not modified. Use conditional workflow triggering to skip building the workflow if the watched files were not updated since the last successful build.

You should specify the files to watch in changeset by using the includes and excludes keys.

    triggering:
      events:
        - push
    when:
      changeset:
        includes:
          - 'lib/'                 #Only triggers when canges are made in lib folder of your repo
        excludes:
          - '**/*.md'              #the build would be skipped if there were changes only to Markdown files .md.
        
 
Skipping a build trigger
In some cases, you may want to make a change to your source code but you don't want to invoke a build. For example, you might not want to invoke a build when you update documentation or configuration files.

In such scenarios, you can include [skip ci] or [ci skip] in the commit message, and a build will not be invoked.
