ARG VARIANT=3.10

# https://www.kenmuse.com/blog/forcing-dev-container-to-use-amd64/
FROM --platform=x86_64 mcr.microsoft.com/devcontainers/python:${VARIANT}-bullseye
# FROM --platform=linux/arm64 mcr.microsoft.com/devcontainers/python:${VARIANT}-bullseye

ENV PYTHONUNBUFFERED 1
ENV DEBIAN_VERSION=11
# Fix for https://github.com/Azure/bicep/issues/10245#issuecomment-1816816017
ENV DOTNET_EnableWriteXorExecute=0

# Install Azure CLI
RUN curl -fsSL https://aka.ms/install-azd.sh | bash
# Set up package feed
RUN wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.asc.gpg
RUN sudo mv microsoft.asc.gpg /etc/apt/trusted.gpg.d/
RUN wget -q https://packages.microsoft.com/config/debian/$DEBIAN_VERSION/prod.list
RUN sudo mv prod.list /etc/apt/sources.list.d/microsoft-prod.list
RUN sudo chown root:root /etc/apt/trusted.gpg.d/microsoft.asc.gpg
RUN sudo chown root:root /etc/apt/sources.list.d/microsoft-prod.list

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive 
RUN apt-get -y install --no-install-recommends libpq-dev postgresql-client
# Install the Azure Functions Core Tools
# https://cloudedarchitecture.com/article/unable-to-locate-package-azure-functions-core-tools-4/
# https://github.com/Azure/azure-functions-core-tools
RUN sudo apt-get install azure-functions-core-tools-4
