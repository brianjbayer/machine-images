# Machine Images
This is a repository of (Docker) *machine images* generally used
for developing and/or running containerized applications.

> :construction_worker_woman: Note that this repository is under
> initial development and is subject to change as experience is
> gained by using it.

## What
A machine image is intended to be the custom operating
environment for a containerized application containing...
* Basic operating system/environment (e.g Debian linux)
* Operating system packages (e.g. `gcclib`, Postgres client)
* Languages (e.g. Ruby, Node)
* Frameworks (e.g. Rails, Express.js)
* Language and/or framework support (e.g. Bundler, Yarn)

The machine image is the *base* image for an application image
and must already be built before building the application.
Note that if an application requires a new additional package
(or any changes to existing packages), it will need an updated
machine image containing that package.

There may be multiple applications using the same machine image.

### Deployment and Development Machine Images
Often applications will have a different image/Dockerfile *layer*
for its deployment (i.e. production) image and its development
environment image and will require different base machine images.
This is because the installed packages and systems (and the actual
application libraries) are different between the application's
build/dev environment and its deployment environment.  For example,
the build/dev environment needs `gcc` and `libpq-dev` to build the
application's libraries but the deploy environment
does not need them and only needs the `postgresql-client `.
For this reason there will often be both a deployment and development
(dev) machine image here.

## Why
Machine images offer two major advantages:
1. Faster application image building
2. Risk mitigation of unintended changes

### Faster Application Image Building
Base machine images offers the advantage of faster building of the
application image.  You are not waiting to fetch, build, and install
packages in the machine portion of the application image.  This will
reduce the overall time spent when developing the application.

### Risk Mitigation of Unintended Changes
In addition to faster application image building, a separate
application-specific Machine image offers the advantage of
minimizing the uncertainty and risk of changes during the
package fetch and build process.  Rebuilding the machine-portion
of the application image can result in slightly different versions
of the installed packages even if the true base image is *pinned*
using the Digest (i.e. image SHA).  This is particularly an issue
with Alpine-based images.

