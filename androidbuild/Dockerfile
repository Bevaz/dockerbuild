FROM shippableimages/ubuntu1404_base

MAINTAINER Bevaz "The.Bevaz@gmail.com"

RUN dpkg --add-architecture i386
RUN echo | add-apt-repository ppa:webupd8team/java
RUN apt-get update
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
RUN apt-get install -y --no-install-recommends p7zip-full unzip python-dev libstdc++6:i386 libgcc1:i386 zlib1g:i386 libncurses5:i386 wget ant oracle-java8-installer zlib1g-dev unzip libssl-dev libpcre3-dev zlib1g-dev libsqlite3-dev xvfb
RUN pip install cython

WORKDIR /opt
RUN wget http://dl.google.com/android/ndk/android-ndk-r10d-linux-x86_64.bin
ADD ndk-exclude.txt /opt/ndk-exclude.txt
RUN 7z x -xr@ndk-exclude.txt android-ndk-r10d-linux-x86_64.bin
ADD statvfs.h /opt/android-ndk-r10d/platforms/android-14/arch-arm/usr/include/sys/statvfs.h

RUN wget http://dl.google.com/android/android-sdk_r24.0.2-linux.tgz
RUN tar xf android-sdk_r24.0.2-linux.tgz
ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools
RUN echo yes | android update sdk -u -a -t platform-tools,build-tools-21.1.2,android-14,sys-img-armeabi-v7a-android-14,android-21,sys-img-armeabi-v7a-android-21
 
ENV ANDROID_AVD_HOME /root/.android/avd/
RUN android create avd --force -n emul-android-4.2 -t android-21 -d tv_1080p -c 512M --abi armeabi-v7a

ADD android-emulator /usr/local/bin/android-emulator
RUN android-emulator start && sleep 60 && android-emulator stop

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*