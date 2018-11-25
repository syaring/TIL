# Use an existing docker images as a base
FROM alpine

# Download and install a dependency
RUN apk add --update redis
RUN apk add --update gcc

# Tell tghe iamge what to do when it stats
# as a container
CMD ["redis-server"]
