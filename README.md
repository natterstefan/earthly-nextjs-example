# Earthly-NextJS-Example

## Commands

```bash
# build image
earth +docker

# start image
docker run --rm -p 3000:3000 earthly-nextjs-example:latest

# debug image
docker run -ti --entrypoint /bin/sh earthly-nextjs-example:latest

# run earth with dev image
earth +dev && docker run --rm -p 3000:3000 \
  -v $PWD/components:/app/components \
  -v $PWD/lib:/app/lib \
  -v $PWD/pages:/app/pages \
  -v $PWD/posts:/app/posts \
  -v $PWD/public:/app/public \
  -v $PWD/styles:/app/styles \
  earthly-nextjs-example-dev:latest
```

## Docs

- <https://www.earthly.dev/>
- <https://github.com/earthly/earthly>
- <https://docs.earthly.dev/guides>
- <https://github.com/earthly/earthly/tree/master/examples/js>
- <https://marketplace.visualstudio.com/items?itemName=earthly.earthfile-syntax-highlighting>
- <https://github.com/vercel/next-learn-starter>
- <https://www.npmjs.com/package/eslint-config-ns>

## LICENSE

MIT
