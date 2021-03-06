# wingedblade — A lightweight, privary friendly analytics platform for underrepresented miniorities

GREATER PURPOSE

NO DONATIONS; SUPPORT ME IN OTHER WAYS

## MVP

Fathom running in a cloud native manner. Designed to be highly portable 

This can be run anywhere assuming there is a Postgres database for which you have a connection string (assuming the host isn't firewalled).

### Environment

Add the following to the root account's environment (.bash_profile):

FSECRET  
FDBSTRG

### Docker Bits

`git clone https://github.com/chris-short/wingedblade.git`

#### Build Container

`docker build --build-arg fsecret=$FSECRET --build-arg fdbstrg=$FDBSTRG -t quay.io/chrisshort/fathom:latest .`

#### Run Container

`docker run -p=8080:8080 quay.io/chrisshort/fathom:latest`


## WingedBlade Proof of Concept

## Purpose

Get a container running with Fathom and a cloud-based PostgreSQL backend. For testing purposes, being exposed to the web for a short time is acceptable.

Should be resilient to restarts

## System Setup

A Fedora 29 box designed to run one Docker container as a proof of concept and alpha version of wingedblade. For immediate use.

## Dependencies

`dnf install git docker vim`

### Testing

`docker pull quay.io/chrisshort/fathom:latest`

`docker run --restart=always --name=systemd_fathom -p=8080:8080 quay.io/chrisshort/fathom:latest`

## Operationalization (Run as systemd Service)

See [fathom.service](fathom.service).

WORD OF CAUTION: Have an edge strategy (load balancing) before potentially exposing this Go service to the internet (this is a POC after all).

```sudo cp fathom.service /etc/systemd/system/ \
    && systemctl daemon-reload \
    && systemctl enable fathom \
    && systemctl start fathom
```

Here's is where the sitcky bits around load balancing come in.

For whatever reason, the Digital Ocean load balancer couldn't go green last night. Then this morning I remembered Digital Ocean load balancers took a little bit of time to have settings changes distribute. Built a new one, configured it according, added it to the fireall on 8080 on fathom001, waited a few minutes... Boom! Success.

The next option would've been Traefik, Let's Encrypt, & Docker 🤮. But, while researching this path I realized I don't necessarily want this for a simple standalone instance. Although, it'd be a little be cheaper it's not worth the time.