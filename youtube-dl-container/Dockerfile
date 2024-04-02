FROM ubuntu:22.04

WORKDIR /mydir

RUN apt-get update && apt-get install -y curl python3
RUN curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
RUN chmod a+x /usr/local/bin/yt-dlp

# This will allow for usage of an argument, since it provides the exucutable that is run when the container is started (and we can give arguments for it)
ENTRYPOINT ["/usr/local/bin/yt-dlp"]ko