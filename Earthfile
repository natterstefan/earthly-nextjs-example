# Earthfile
#
# Docs:
# - https://docs.earthly.dev/guides/basics
# - https://docs.earthly.dev/guides/basics#detailed-explanation
FROM node:14.13.1-alpine3.12
WORKDIR /app

# https://docs.earthly.dev/guides/basics#reduce-code-duplication
deps:
    COPY package.json ./
    COPY package-lock.json ./
    RUN npm install

    SAVE ARTIFACT package.json AS LOCAL ./package.json
    SAVE ARTIFACT package-lock.json AS LOCAL ./package-lock.json
    SAVE IMAGE

source:
    FROM +deps
    COPY components components
    COPY lib lib
    COPY pages pages
    COPY posts posts
    COPY styles styles
    SAVE IMAGE

dev:
    FROM +source
    CMD ["npm", "run", "dev"]
    SAVE IMAGE earthly-nextjs-example-dev:latest

# https://docs.earthly.dev/guides/docker-in-earthly
# https://docs.earthly.dev/earthfile#with-docker-beta
# run: earth -P +rundev
# rundev:
#     FROM docker:19.03.12-dind
#     WITH DOCKER
#         DOCKER LOAD +dev earthly-nextjs-example-dev:latest
#         RUN docker run --network=host \
#             -v $PWD/components:/app/components \
#             -v $PWD/lib:/app/lib \
#             -v $PWD/pages:/app/pages \
#             -v $PWD/posts:/app/posts \
#             -v $PWD/public:/app/public \
#             -v $PWD/styles:/app/styles \
#             earthly-nextjs-example-dev:latest
#     END

lint:
    FROM +source
    COPY prettier.config.js prettier.config.js
    COPY .eslintrc.js .eslintrc.js
    COPY .eslintcache .eslintcache

    RUN npm run lint

    SAVE ARTIFACT .eslintcache AS LOCAL .eslintcache

# Declare a target, build.
build:
    FROM +source

    RUN npm run build
    SAVE ARTIFACT .next /nextjs AS LOCAL ./.next

# Declare a target, docker.
docker:
    COPY package.json package-lock.json ./
    RUN npm install

    # Copy the artifact /dist produced by another target, +build, to the
    # current directory within the build container.
    COPY +build/nextjs .next
    COPY public public

    # Set the entrypoint for the resulting docker image.
    CMD ["npm", "start"]

    # Save the current state as a docker image, which will have the docker tag
    # earthly-nextjs-example:latest. This image is only made available to the
    # host's docker if the entire build succeeds.
    SAVE IMAGE earthly-nextjs-example:latest