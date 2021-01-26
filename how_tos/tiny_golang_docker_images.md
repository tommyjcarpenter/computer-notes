# tiny golang images

Blogs followed

1. https://blog.kloia.com/micro-docker-images-for-go-applications-8a8701130c01
2. https://medium.com/@chemidy/create-the-smallest-and-secured-golang-docker-image-based-on-scratch-4752223b7324
3. https://blog.codeship.com/building-minimal-docker-containers-for-go-applications/

NOTE: haven’t hit this yet, but see this GitHub issue: https://github.com/golang/go/issues/9344#issuecomment-69944514

SO post got from:
https://stackoverflow.com/questions/47837149/build-docker-with-go-app-cannot-find-package

So: there appears to be a build flag for go(lang) applications that invokes a static compile and linking.
I also (separate from go) learned that you can have a “multi stage dockerfile” where you build one image, then build another, copying something out of the first image.

 
Example Dockerfile:

```
# First stage to compile and statically link the app
FROM golang:latest AS builder
ADD . /go/src/golearn
WORKDIR /go/src/golearn
RUN go get .
RUN CGO_ENABLED=0 GOOS=linux  go build -a
RUN ls /go/bin
ENTRYPOINT ["/go/bin/golearn"]

# second stage to obtain a very small image
FROM scratch
COPY --from=builder /go/bin/golearn .
CMD ["./golearn"]
```

 
This two stage Dockerfile results in a 2MB (!!!) Docker image that runs my application (called golearn):

 
```
docker build -t gotest:0.0.1 .
docker images                                                                                                                                

REPOSITORY                             TAG                 IMAGE ID            CREATED              SIZE

gotest                                       0.0.1               0dea965edea8        5 minutes ago        2.01MB
```
