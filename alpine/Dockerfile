FROM node:14.17-alpine3.13 as build

WORKDIR /home/node

RUN apk add --no-cache git python2 g++ make bash

#fix error - url loader
RUN npm install url-loader --save-dev
 
# Clone git and remove .git
RUN git clone -b master --depth 1 https://github.com/vatesfr/xen-orchestra/

# patches
COPY patches /home/node/xen-orchestra/patches
RUN cd /home/node/xen-orchestra \
    && git apply patches/gh_issue_redirect.diff \
    && rm -rf /home/node/xen-orchestra/.git /home/node/xen-orchestra/patches

# build
WORKDIR /home/node/xen-orchestra
RUN ls -la
RUN yarn config set network-timeout 300000
RUN yarn
RUN yarn build

# plugins
COPY link_plugins.sh /home/node/xen-orchestra/packages/xo-server/link_plugins.sh
RUN /home/node/xen-orchestra/packages/xo-server/link_plugins.sh

