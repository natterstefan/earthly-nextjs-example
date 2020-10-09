# Earthfile
#
# Docs:
# - https://docs.earthly.dev/guides/basics
# - https://docs.earthly.dev/guides/basics#detailed-explanation
FROM node:14.13.1-alpine3.12
WORKDIR /app

# Declare a target, build.
build:
    COPY package.json package-lock.json ./
    COPY components components
    COPY lib lib
    COPY pages pages
    COPY posts posts
    COPY styles styles

    RUN npm install

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