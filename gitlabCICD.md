**[< Home](README.md)**
# GitLab CI/CD
All that in a file `.gitlab-ci.yml` on a repertory pushed on gitlab.

Job 
- some script to run 
- conditional execution

Stage 
- contains jobs
- allows parallel job


Ex of stage/job gestion in `.gitlab-ci.yml` :
```yaml
stages: # Declared stage order is execution order
    - build
    - test

# Jobs definition
compile:
    stage: build
    script:
    - echo "Building..."

unit:
    stage: test
    script:
        - echo "Unit tests..."
        - echo "You can put multiple commands in one job"

integration:
    stage: test
    script:
        - echo "Integration tests"
```

On docker :
```yaml
unit:
    image: maven:3.6.1-jdk-12
    stage: test
    script:
        - mvn test -f cdb-app
```


# Sercret and variables
```yaml
variables:
MY_VARIABLE: "foo"
...
deploy:
    stage: deploy
    script:
        - echo $MY_VARIABLE # .gitlab-ci.yml variable
        - echo $MY_SECRET # gitlab secret variable
        - echo $CI_JOB_ID # predefined gitlab variable
```

We can have different env

```yaml
deploy:dev:
    stage: deploy
    environment: dev
    script:
        - echo $MY_SECRET # .gitlab-ci.yml variable

deploy:prod:
    stage: deploy
    environment: prod
    script:
        - echo $MY_SECRET # .gitlab-ci.yml variable
```

We can choose the branch :
```yaml
deploy:
    stage: deploy
    script:
        - echo "This job only runs for commits on main"
    rules:
        - if: ‘$CI_COMMIT_REF_NAME == “main”
```

Workflow to rule them all : 
```yaml
workflow:
    rules:
        - if: ‘$CI_COMMIT_REF_NAME == “develop”’
            when: never
        - if: $CI_COMMIT_BRANCH

deploy:
    stage: deploy
    script:
        - echo "This job only runs for commits on main"
```
We can put `$CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH` to check if it's the main branch

Or `$CI_COMMIT_BRANCH =~ "/^feature/"` to match a regex
Or `$CI_PIPELINE_SOURCE == "merge_request_event"` 

## Cache 
```yaml
# Cache for every job in every pipeline
cache:
    paths:
        - .m2/repository

variables:
    MAVEN_OPTS:
        "-Dmaven.repo.local=$CI_PROJECT_DIR/.m2/repository"

...

unit:
    image: maven:3.6.1-jdk-12
    stage: test
    script:
        - mvn test -f cdb-app
```
Mettre une cle pour le cache c'est l'equivalent de connaitre l'id (c'est mieux)

## Artifacts 
```yaml
stages:
    - build
    - deploy

compile:
    stage: build
    # Pass artifacts to specific job
    artifacts:
        paths:
            - artifact.txt
    script:
        - echo "Building..." >> artifact.txt

deploy:
    stage: deploy
    dependencies:
        – compile
    script:
        - cat artifact.txt
```

## Hidden job 
We can create hidden job (with a `.` at the start of the job name), this type of job aren't executed but can serve as template for other.
```yaml
.mvn: &mvn_base
  image: ${MAVEN_IMAGE}
  cache:
    paths:
      - ...
    policy: pull-push

compile:
  <<: *mvn_base
  stage: build
  script:
    - ...

test:
  <<: *mvn_base
  stage: test
  script:
    - ...
```
Or with extend
```yaml
.mvn:
  image: ${MAVEN_IMAGE}
  cache:
    paths:
      - ...
    policy: pull-push

compile:
  extends: .mvn
  stage: build
  script:
    - ...

test:
  extends: .mvn
  stage: test
  script:
    - ...
```
*Le job de prod ne doit dependre qu'aucun autre*

## `Services`
We can start a docker on witch depend the actual job with `services` (Ex: a database for integration test)

in a job it's defined like this : 
```yaml
    services:
        - name: mysql:8
        alias: db
    
    variables:
        MYSQL_DATABASE: "computer-database-db"
        MYSQL_ROOT_PASSWORD: "root"
        MYSQL_USER: "cdb-user"
        MYSQL_PASSWORD: "cdb-password"
```

# Runner 
Set up a runner
```bash
docker run \
  -d \
  --name gitlab-runner \
  --restart always \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --cpus="1" \
  --memory="2g" \
  gitlab/gitlab-runner
```

And register it.
Navigate to your repository on Gitlab, and go to Settings > CI / CD > Runners to get the `registration token`

Then on local machine configure the runner
```bash
docker run \
  --rm \
  -it \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  gitlab/gitlab-runner \
  register
```
