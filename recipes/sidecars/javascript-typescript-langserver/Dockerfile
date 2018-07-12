# inherit an image with node.js and npm - runtime requirements for TypeScript LS
FROM eclipse/node

# install globally to avoid the install in our workspace directory
RUN sudo npm i -g javascript-typescript-langserver

RUN sudo apt-get update && \
    sudo apt-get install socat -y

CMD socat TCP4-LISTEN:3334,reuseaddr,fork EXEC:"node /usr/lib/node_modules/javascript-typescript-langserver/lib/language-server-stdio"
