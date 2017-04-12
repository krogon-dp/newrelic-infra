# newrelic-infra-docker

WIP `Dockerfile` for hosting the newrelic-infra agent in a docker container.


## Building

A simple `docker build` should be sufficient from within the root of the repo.

    % docker build .
    Sending build context to Docker daemon  61.44kB
    Step 1/11 : FROM centos:7
     ---> a8493f5f50ff
    Step 2/11 : MAINTAINER Robin Kearney <robin@kearney.co.uk>
     ---> Using cache
     ---> 212e502460f8
    Step 3/11 : ADD newrelic-infra.repo /etc/yum.repos.d/
     ---> Using cache
     ---> 8da4b462fe85
    Step 4/11 : RUN curl -o /etc/yum.repos.d/newrelic-infra.repo https://download.newrelic.com/infrastructure_agent/linux/yum/el/7/x86_64/newrelic-infra.repo
     ---> Using cache
     ---> f9adacc02af2
    Step 5/11 : RUN yum --nogpgcheck makecache fast &&     yum -y --nogpgcheck install newrelic-infra dmidecode policycoreutils &&     yum clean all
     ---> Using cache
     ---> e338ecddac52
    Step 6/11 : ADD newrelic-infra.yml /etc/
     ---> Using cache
     ---> 8276adf316a1
    Step 7/11 : ENV NRIA_LOGLEVEL "info"
     ---> Using cache
     ---> eda8032f8a1a
    Step 8/11 : ENV NRIA_VERBOSE 0
     ---> Using cache
     ---> b1fd39c698bc
    Step 9/11 : ENV NRIA_OVERRIDE_HOST_ROOT "/mnt/ROOT"
     ---> Using cache
     ---> a22e328a61a9
    Step 10/11 : ENV NRIA_LICENSE_KEY "null"
     ---> Using cache
     ---> 4c67e0e01896
    Step 11/11 : CMD /usr/bin/newrelic-infra -config /etc/newrelic-infra.yml
     ---> Using cache
     ---> 59b7a117a15e
    Successfully built 59b7a117a15e

## Running

### Manually using Docker

The below example assumes you've set the `NRIA_LICENSE_KEY` environment variable to your license key before you run `docker run`.

    docker run \
           --rm \
           --uts=host \
           --pid=host \
           --net=host \
           --privileged=true \
           -e NRIA_LICENSE_KEY="$NRIA_LICENSE_KEY" \
           -v /:/mnt/ROOT:ro \
           rk295-newrelic-infra


### Running from inside Kubernetes

TODO
