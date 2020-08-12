FROM ubuntu:18.04

RUN apt-get update && apt-get install inotify-tools -y && apt-get install openssl -y && apt-get install -y dos2unix

WORKDIR /tagion/

COPY . .
RUN dos2unix /tagion/devnet/bootstrap && dos2unix /tagion/devnet/demo_send && dos2unix /tagion/devnet/devnet 

RUN chmod +x /tagion/wallet/tagionwallet && chmod +x /tagion/utils/hibon/hibonutil && chmod +x /tagion/utils/dart/dartutil && chmod +x /tagion/utils/tagion/tagionutil && chmod +x /tagion/devnet/tagionwave

ENV GODEBUG=cgocheck=0
ENV PATH="/tagion/devnet:/tagion/wallet:/tagion/utils/dart:/tagion/utils/hibon:/tagion/utils/tagion:${PATH}"


WORKDIR /workspace/

CMD bash