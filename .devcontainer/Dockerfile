# Start from the official .NET devcontainer base (Debian Bookworm)
FROM mcr.microsoft.com/devcontainers/dotnet:1-8.0-bookworm

# ── Install .NET 10 SDK on top ──────────────────────────────────────────────
COPY --from=mcr.microsoft.com/dotnet/nightly/sdk:10.0 /usr/share/dotnet /usr/share/dotnet

# ── System dependencies ──────────────────────────────────────────────────────
RUN apt-get update && apt-get install -y \
    curl wget unzip zip git \
    openjdk-17-jdk \
    libglib2.0-0 libgl1 libx11-dev \
    --no-install-recommends \
 && rm -rf /var/lib/apt/lists/*

# ── Android SDK (command-line tools) ────────────────────────────────────────
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV ANDROID_HOME=/opt/android-sdk
ENV PATH="${PATH}:${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin:${ANDROID_SDK_ROOT}/platform-tools"

ARG CMDTOOLS_URL=https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip

RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools \
 && wget -q "${CMDTOOLS_URL}" -O /tmp/cmdtools.zip \
 && unzip -q /tmp/cmdtools.zip -d /tmp/cmdtools \
 && mv /tmp/cmdtools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/latest \
 && rm -rf /tmp/cmdtools /tmp/cmdtools.zip

# Accept licences and install required Android SDK packages
RUN yes | sdkmanager --licenses > /dev/null 2>&1 \
 && sdkmanager \
    "platform-tools" \
    "platforms;android-35" \
    "build-tools;35.0.0"

# ── .NET MAUI workload ────────────────────────────────────────────────────────
# Pin to .NET 10 SDK (make sure dotnet 10 is selected)
RUN dotnet --version \
 && dotnet workload install maui-android --skip-sign-check
