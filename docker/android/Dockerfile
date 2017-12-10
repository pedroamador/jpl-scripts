FROM ubuntu:17.10
ENV DEBIAN_FRONTEND noninteractive

# Based on Eric Chang docker jenkins android https://github.com/chiahan1123/docker-jenkins-android
LABEL maintainer="Pedro Amador Rodríguez <pedroamador.rodriguez@gmail.com>"

# Android SDK environment variables
# Command line tools file from
# https://developer.android.com/studio/index.html
ENV ANDROID_SDK_HOME /usr/local/.android
ENV ANDROID_HOME /usr/local/android-sdk-linux
ENV ANDROID_TOOLS_FILE sdk-tools-linux-3859397.zip
ENV PATH $PATH:${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/platform-tools
RUN echo ANDROID_HOME="${ANDROID_HOME}" >> /etc/environment

# Gradle environment variables
ENV GRADLE_USER_HOME /usr/local/.gradle
ENV GRADLE_DEFAULT_VERSION 3.5
ENV GRADLE_DEFAULT_FILE gradle-${GRADLE_DEFAULT_VERSION}-bin.zip
ENV PATH $PATH:${GRADLE_USER_HOME}/gradle-${GRADLE_DEFAULT_VERSION}/bin

# Install packages
RUN apt-get -y update && \
    dpkg --add-architecture i386 && \
    apt-get -y install openjdk-8-jdk && \
    apt-get -y install git wget curl unzip make g++ ruby ruby-dev locales libqt5widgets5 unzip && \
    apt-get install -y expect ant libc6-i386 lib32stdc++6 lib32gcc1 lib32ncurses5 lib32z1 && \
    apt-get clean && \
    apt-get autoclean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Locale settings
RUN locale-gen en_US.UTF-8

ENV LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    LC_ALL=en_US.UTF-8

# Install fastlane
RUN gem install fastlane -NV

# Installing Default Gradle
WORKDIR $GRADLE_USER_HOME
RUN wget -q https://services.gradle.org/distributions/${GRADLE_DEFAULT_FILE} && \
    unzip $GRADLE_DEFAULT_FILE && \
    rm $GRADLE_DEFAULT_FILE

# Installing Android SDK
WORKDIR $ANDROID_HOME
RUN mkdir -p /root/.android && \
    touch /root/.android/repositories.cfg && \
    wget -q https://dl.google.com/android/repository/${ANDROID_TOOLS_FILE} && \
    unzip $ANDROID_TOOLS_FILE && \
    rm $ANDROID_TOOLS_FILE && \
    echo y | sdkmanager "platform-tools" "tools" "extras;android;m2repository" "extras;google;m2repository" "extras;google;google_play_services"

RUN chmod 777 -R /var/lib/gems/2.3.0 /home

# Work directory
WORKDIR /home
