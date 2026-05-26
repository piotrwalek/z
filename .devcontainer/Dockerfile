FROM mcr.microsoft.com/devcontainers/dotnet:1-8.0-bookworm

# ── .NET 10 SDK ──────────────────────────────────────────────────────────────
COPY --from=mcr.microsoft.com/dotnet/nightly/sdk:10.0 /usr/share/dotnet /usr/share/dotnet

# ── Usuń zepsute repo Yarn, które blokuje apt-get update ─────────────────────
RUN rm -f /etc/apt/sources.list.d/yarn.list \
 && rm -f /usr/share/keyrings/yarnkey.gpg \
 && apt-get update && apt-get install -y \
    curl wget unzip zip git \
    openjdk-17-jdk \
    libglib2.0-0 libgl1 libx11-dev \
    --no-install-recommends \
 && rm -rf /var/lib/apt/lists/*

# ── Android SDK ───────────────────────────────────────────────────────────────
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV ANDROID_HOME=/opt/android-sdk
ENV PATH="${PATH}:${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin:${ANDROID_SDK_ROOT}/platform-tools"

ARG CMDTOOLS_URL=https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip

RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools \
 && wget -q "${CMDTOOLS_URL}" -O /tmp/cmdtools.zip \
 && unzip -q /tmp/cmdtools.zip -d /tmp/cmdtools \
 && mv /tmp/cmdtools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/latest \
 && rm -rf /tmp/cmdtools /tmp/cmdtools.zip

RUN yes | sdkmanager --licenses > /dev/null 2>&1 \
 && sdkmanager \
    "platform-tools" \
    "platforms;android-35" \
    "build-tools;35.0.0"

# ── MAUI workload ─────────────────────────────────────────────────────────────
RUN dotnet workload install maui-android --skip-sign-check
