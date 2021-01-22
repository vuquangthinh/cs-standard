## This is config for gitlab ci/cd
- Gitlab ci/cd is a tool of Gitlab repo. Easy to use and config.
- File ```.gitlab-ci.yml``` is basic config step of gitlab ci/cd.


```
stages:
  - build
  - deploy

before_script:
  - |
    # docker variables for name and tag of new image
    export DOCKER_TAG="${CI_COMMIT_SHA:0:8}"
    export DOCKER_REPO="$CI_REGISTRY_IMAGE"
    export DOCKER_IMAGE="${DOCKER_REPO}:${DOCKER_TAG}"
    export DOCKER_CACHE_IMAGE="${DOCKER_REPO}:${CI_COMMIT_REF_NAME}"
# Build of docker image and push it to project Container registry
build:
  # Lightweight image with support of running docker in docker
  image: docker:stable
  services:
    - docker:dind
  stage: build
  script:
    # Login to Container registry
    - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" "$CI_REGISTRY"
    # Load existing images for build optimisation
    - docker pull "$DOCKER_CACHE_IMAGE" || docker pull "${DOCKER_REPO}:deploy" || true # allow failure
    - docker pull "$DOCKER_IMAGE" || true # allow failure
    # Build our docker image
    - docker build --pull --cache-from "$DOCKER_IMAGE" --cache-from "$DOCKER_CACHE_IMAGE" --cache-from "${DOCKER_REPO}:deploy" -t "$DOCKER_IMAGE" .
    # Save built image to Container registry under branch name tag and commit hash tag
    - docker push "$DOCKER_IMAGE"
    - docker tag "$DOCKER_IMAGE" "$DOCKER_CACHE_IMAGE"
    - docker push "$DOCKER_CACHE_IMAGE"
  only:
    - deploy

deploy:
  # Lightweight image with ssh
  image: kroniak/ssh-client
  stage: deploy
  script:
    # Set right chmod on SSH key file
    - chmod 400 $MASTER_SSH_KEY
    # Login to Gitlab Container registry
    - ssh -o StrictHostKeyChecking=no -i $MASTER_SSH_KEY "${MASTER_SSH_USER}@${MASTER_HOST}" "sudo docker login -u ${CI_DEPLOY_USER} -p ${CI_DEPLOY_PASSWORD} ${CI_REGISTRY}"
    # Remove old containers and images if exists
    - ssh -o StrictHostKeyChecking=no -i $MASTER_SSH_KEY "${MASTER_SSH_USER}@${MASTER_HOST}" "sudo docker rm -f ${CI_PROJECT_NAME} || true"
    - ssh -o StrictHostKeyChecking=no -i $MASTER_SSH_KEY "${MASTER_SSH_USER}@${MASTER_HOST}" "sudo docker rmi \$(sudo docker images -q ${DOCKER_REPO}) || true"
    # Download and run new image
    - ssh -o StrictHostKeyChecking=no -i $MASTER_SSH_KEY "${MASTER_SSH_USER}@${MASTER_HOST}"
      sudo docker run
      --name=$CI_PROJECT_NAME
      -v "/app/storage:/src/storage"
      -p 5001:3000
      -d $DOCKER_IMAGE
  only:
    - deploy
```

### Explain:
- We define 2 step: build, deploy.
- before script: define variable docker new image.
- only: define your branch you want build
- we need: add variable $MASTER_SSH_KEY, $MASTER_HOST, $MASTER_SSH_USER in ``` Settings -> CI/CD -> Variables ```.
- Push to gitlab and enjoy.
