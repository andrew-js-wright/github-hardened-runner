# use no externals to get rid of nodejs
FROM mcr.microsoft.com/dotnet/runtime-deps:7.0-jammy as build

ARG RUNNER_VERSION="2.301.1"
ARG RUNNER_ARCH="x64"
ARG RUNNER_CONTAINER_HOOKS_VERSION=0.2.0

RUN apt update -y && apt install curl unzip -y && apt autoremove -y

WORKDIR /actions-runner
RUN curl -k -f -L -o runner.tar.gz https://github.com/actions/runner/releases/download/v${RUNNER_VERSION}/actions-runner-linux-${RUNNER_ARCH}-${RUNNER_VERSION}-noexternals.tar.gz \
    && tar xzf ./runner.tar.gz \
    && rm runner.tar.gz

RUN curl -k -f -L -o runner-container-hooks.zip https://github.com/actions/runner-container-hooks/releases/download/v${RUNNER_CONTAINER_HOOKS_VERSION}/actions-runner-hooks-k8s-${RUNNER_CONTAINER_HOOKS_VERSION}.zip \
    && unzip ./runner-container-hooks.zip -d ./k8s \
    && rm runner-container-hooks.zip


FROM mcr.microsoft.com/dotnet/runtime-deps:7.0-jammy

ENV RUNNER_ALLOW_RUNASROOT=1
ENV RUNNER_MANUALLY_TRAP_SIG=1
ENV ACTIONS_RUNNER_PRINT_LOG_TO_STDOUT=1

WORKDIR /actions-runner
COPY --from=build /actions-runner .
