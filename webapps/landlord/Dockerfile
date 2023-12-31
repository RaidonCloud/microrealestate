FROM node:18-alpine

RUN apk --no-cache add build-base python3

WORKDIR /usr/app

COPY package.json .
COPY yarn.lock .
COPY .yarnrc.yml .
COPY .yarn .yarn
COPY .eslintrc.json .
COPY webapps/commonui webapps/commonui
COPY webapps/landlord/public webapps/landlord/public
COPY webapps/landlord/locales webapps/landlord/locales
COPY webapps/landlord/src webapps/landlord/src
COPY webapps/landlord/.eslintrc.json webapps/landlord
COPY webapps/landlord/i18n.js webapps/landlord
COPY webapps/landlord/next.config.js webapps/landlord
COPY webapps/landlord/package.json webapps/landlord
COPY webapps/landlord/LICENSE webapps/landlord


ENV NEXT_TELEMETRY_DISABLED=1

# base path cannot be set at runtime: https://github.com/vercel/next.js/discussions/41769
ARG LANDLORD_BASE_PATH
ENV BASE_PATH=$LANDLORD_BASE_PATH
ENV NEXT_PUBLIC_BASE_PATH=$LANDLORD_BASE_PATH

RUN corepack enable && \
    corepack prepare yarn@stable --activate

RUN yarn workspaces focus @microrealestate/landlord && \
    yarn workspace @microrealestate/landlord run build

# TODO: check why using user node is failing
# RUN chown -R node:node /usr/app

# USER node

CMD yarn workspace @microrealestate/landlord run generateRuntimeEnvFile && \
    yarn workspace @microrealestate/landlord start -p $PORT