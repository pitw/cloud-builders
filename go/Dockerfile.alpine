FROM golang:alpine

# Install VCS tools to support "go get" commands and install gcc.
RUN apk add --update --no-cache git mercurial subversion build-base

# Replace for Bitbucket Private Repos
RUN git config --global url."git@bitbucket.org:".insteadOf "https://bitbucket.org/"

# To be sure also replace api.bitbucket.org
RUN git config --global url."git@bitbucket.org:".insteadOf "https://api.bitbucket.org/"

# We blank out the GOPATH because the base image sets it, and
# if the user of this build step does *not* set it, we want to
# explore other options for workspace derivation.
ENV GOPATH=

RUN mkdir /builder

COPY go_workspace.go prepare_workspace.inc /builder/

COPY go.ash /builder/bin/
ENV PATH=/builder/bin:$PATH

RUN go build -o /builder/go_workspace /builder/go_workspace.go

ENTRYPOINT ["go.ash"]
