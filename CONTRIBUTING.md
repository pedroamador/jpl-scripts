# Contributing

## Issues

Issues are always very welcome - after all. However, there are a couple of things you can do to make the lives of the developers _much, much_ easier:

### Tell us

* What you are doing?
  * Post a _minimal_ code sample that reproduces the issue
  * What do you expect to happen?
  * What is actually happening?
* Which kind of pipeline are you using: _scripted_ or _declarative_
* Which Red Panda JPL version are you using?

When you post code, please use [Github flavored markdown](https://help.github.com/articles/github-flavored-markdown), in order to get proper syntax highlighting!

If you can even provide a pull request with a failing unit test, we will love you long time! Plus your issue will likely be fixed much faster.

## Pull requests

We're glad to get pull request if any functionality is missing or something is buggy. However, there are a couple of things you can do to make life easier for the maintainers:

* Explain the issue that your PR is solving - or link to an existing issue
* Make sure that all existing tests pass
* Make sure you followed [coding guidelines](https://github.com/red-panda-ci/jpl-scripts/blob/master/CONTRIBUTING.md#coding-guidelines)
* Add some tests for your new functionality or a test exhibiting the bug you are solving. Ideally all new tests should not pass _without_ your changes.

Still interested? Coolio! Here is how to get started:

### 1. Prepare your environment

Makes sure `docker` is installed.

If running on macOS, install [Docker for Mac](https://docs.docker.com/docker-for-mac/).

Now pull `redpandaci/jenkins-dind` docker image

```sh
docker pull redpandaci/jenkins-dind
```

### 2. Run the tests

All tests scripts and jobs are located in the `test` folder, and are executed with "bin/test.sh" script within `redpandaci/jenkins-dind` docker container

You can run all tests in your workstations as if they run in Jenkins. These tests are executed in `redpandaci/jenkins-dind` docker image <https://hub.docker.com/r/redpandaci/jenkins-dind/> and uses a docker container with a time box run of 300 seconds. After the tests, or reaching the timeout, the container will be destroyed

```console
$ bin/test.sh
# Start jenkins as a time-boxed daemon container, running for max 300 seconds with id 
[...]
```

You can use "local test" options for the test cript. In this case, the container open "8080" port to jenkins-dind with "redpanda/redpanda" credentials, so you can open the Jenkins in a web browser in <http://localhost:8080>

After the test, the container will not be destroyed, so you must destroy it by yourself with "docker rm -f CONTAINER_ID"

With "local test" options the script will test the scripts that you're working on.

```console
$ bin/test.sh local test
# Start jenkins as a time-boxed daemon container, running for max 0 seconds with id 525dc9e86eb6abae8fbda933eb94fe4c8e463f761ae24c9a2dd5f16d2b09c9b5
[...]
$ echo $?
0
$ docker ps -a
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                            NAMES
525dc9e86eb6        redpandaci/jenkins-dind   "timeout 0 /usr/bi..."   2 minutes ago       Up About a minute   22/tcp, 0.0.0.0:8080->8080/tcp   stupefied_keller
$ docker kill stupefied_keller 
stupefied_keller
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

### 3. Check the documentation

There is a `make` command to generate the main README.md file that you can run (in the jpl dir)

```sh
make
```

The generated documentation will be placed in the `README.md` file, overwriting it. Then you can do a commit to the repository

### 4. Commit

Red Panda JPL scripts follows the [eslint Commit Message Conventions](https://github.com/willsoto/validate-commit/blob/master/conventions/eslint.md).
Example:

```sh
git commit -m "Update: Resolve some stuff"
```

Commit messages are used to automatically generate a changelog, so make sure to follow the convention.
When you commit, your commit message might be validated automatically with [validate-commit-msg](https://github.com/willsoto/validate-commit), sou you can check this [recipie](https://gist.github.com/pedroamador/6635a4912546301c6beca6efd4dc3655).

Then push and send your pull request. Happy hacking and thank you for contributing.

### Coding Guidelines

As part of the test process, all commit messages will be linted, and your PR will **not** be accepted if it does not pass linting.

## Publishing a new release

1. Ensure that latest build on master is green
1. Ensure your local code is up to date (`git pull origin master`)
1. Create a new release with git flow and the `major(X) minor(Y) patch(Z)` (`git flow release start vX.Y.Z`) (see [Semantic Versioning](http://semver.org))
1. Publish the release `git flow feature publish vX.Y.Z`

The rest of the work will be completed within Jenkins, following the `Jenkinfile` pipeline script.
