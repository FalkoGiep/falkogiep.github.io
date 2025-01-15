---
title: Azure Pipelines Cheatsheet
layout: post
category: Jekyll
---

```yaml
stages: [ stage | template ]
pool: pool # Pool where jobs in this pipeline will run unless otherwise specified
name: string # Pipeline run number.. 
trigger: trigger # Continuous integration triggers
parameters: [ name ]
pr: pr # Pull request triggers
schedules: [ cron ]
resources:  # Containers and repositories used in the build
  builds: [ build ]
  containers: [ container ]
  pipelines: [ pipeline ]
  repositories: [ repository ]
  webhooks: [ webhook ]
  packages: [ package ]
variables: variables # Variables for this pipeline
lockBehavior: string # Behavior lock requests from this stage should exhibit in relation to other exclusive lock requests.  (runLatest,sequential)
```

```yaml
stages:
- stage: string  # name of the stage, A-Z, a-z, 0-9, and underscore
  displayName: string  # friendly name to display in the UI
  dependsOn: string | [ string ]
  condition: string
  pool: string | pool
  variables: { string: string } | [ variable | variableReference ] 
  jobs: [ job | templateReference]
```

```yaml
jobs:
- job: string  # name of the job, A-Z, a-z, 0-9, and underscore
  displayName: string  # friendly name to display in the UI
  dependsOn: string | [ string ]
  condition: string
  strategy:
    parallel: # parallel strategy
    matrix: # matrix strategy
    maxParallel: number # maximum number simultaneous matrix legs to run
    # note: `parallel` and `matrix` are mutually exclusive
    # you may specify one or the other; including both is an error
    # `maxParallel` is only valid with `matrix`
  continueOnError: boolean  # 'true' if future jobs should run even if this job fails; defaults to 'false'
  pool: pool # agent pool
  workspace:
    clean: outputs | resources | all # what to clean up before the job runs
  container: containerReference # container to run this job inside
  timeoutInMinutes: number # how long to run the job before automatically cancelling
  cancelTimeoutInMinutes: number # how much time to give 'run always even if cancelled tasks' before killing them
  variables: { string: string } | [ variable | variableReference ] 
  steps: [ script | bash | pwsh | powershell | checkout | task | templateReference ]
  services: { string: string | container } # container resources to run as a service container
  uses: # Any resources (repos or pools) required by this job that are not already referenced
    repositories: [ string ] # Repository references to Azure Git repositories
    pools: [ string ] # Pool names, typically when using a matrix strategy for the job
```

```yaml
variables:
  staticVar: 'my value' # static variable
  compileVar: ${{ variables.staticVar }} # compile time expression
  isMain: $[eq(variables['Build.SourceBranch'], 'refs/heads/main')] # runtime expression

steps:
  - script: |
      echo ${{variables.staticVar}} # outputs my value
      echo $(compileVar) # outputs my value
      echo $(isMain) # outputs True
```
## Sources/Inspiration
- [Cheatsheet by nickkell](https://nickkell.medium.com/azure-devops-pipeline-cheatsheet-b7401f8eb2e7)
- [Expressions](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/expressions?view=azure-devops)
- [Templates](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops&pivots=templates-includes)
- [Set variables in script](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/set-variables-scripts?view=azure-devops&tabs=bash)
