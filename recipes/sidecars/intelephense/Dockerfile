# inherit an image with node.js and npm - runtime requirements for TypeScript LS
FROM eclipse/node

# install globally to avoid the install in our workspace directory
RUN sudo npm i -g intelephense-server

RUN sudo apt-get update && \
    sudo apt-get install socat -y

# create a working directory so that intelephense does not drop its files on our workspace
RUN mkdir /home/user/intelephense

# Very hacky but since intelephense-server expects a non-standard initialize request we patch the code here to use our working directory by deafult
RUN sed -i "s#params.initializationOptions.storagePath#'/home/user/intelephense'#g" /usr/lib/node_modules/intelephense-server/lib/server.js && \
    sed -i "s#clearCache: params.initializationOptions.clearCache#undefined#g" /usr/lib/node_modules/intelephense-server/lib/server.js

# switch to working directory and start the server
CMD socat TCP4-LISTEN:3333,reuseaddr,fork EXEC:"node /usr/lib/node_modules/intelephense-server --stdio"

