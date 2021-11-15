FROM debian:stretch
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -qy openjdk-8-jdk-headless git make python python-pip python-setuptools python-dev build-essential swig pcscd pcsc-tools libccid libpcsclite-dev python-pyscard python-crypto wget
WORKDIR /root
RUN git clone git://git.osmocom.org/sim/sim-tools
RUN git clone git://git.osmocom.org/sim/hello-stk
RUN pip install 
WORKDIR /root/hello-stk/hello-stk
RUN make && ls -la ./build/javacard/org/toorcamp/HelloSTK/javacard/HelloSTK.cap
WORKDIR /root 
RUN mkdir -p /root/.cache/
WORKDIR /root/sim-tools/shadysim
RUN  cp ../../hello-stk/hello-stk/build/javacard/org/toorcamp/HelloSTK/javacard/HelloSTK.cap ./
ADD entrypoint.sh /root/sim-tools/shadysim/entrypoint.sh
RUN chmod ugo+x /root/sim-tools/shadysim/entrypoint.sh
ENTRYPOINT ["/root/sim-tools/shadysim/entrypoint.sh"]
