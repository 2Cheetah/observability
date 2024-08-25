FROM python:latest

RUN apt update && apt install -y curl zsh git

RUN curl -fsSL https://deb.nodesource.com/setup_22.x -o nodesource_setup.sh

RUN bash nodesource_setup.sh

RUN apt install -y nodejs

