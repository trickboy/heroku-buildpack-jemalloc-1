# heroku-buildpack-jemalloc

This Heroku buildpack installs [jemalloc](http://jemalloc.net/) on your dynos. jemalloc is an alternative implementation of `malloc(3)`, a low-level C function that allocates memory. Projects that swap in jemalloc often see a reduction in memory fragmentation and overall memory use.

jemalloc works automatically with Heroku deployments for Ruby, Node, Python, and other languages that manage memory for you.

This buildpack supports both Heroku’s `cedar-14` and `heroku-16` stacks.

## Install

[Heroku supports using multiple buildpacks for an app](https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app):

```bash
heroku buildpacks:add --index 1 https://github.com/scryfall/heroku-buildpack-jemalloc.git
git push heroku master
```

## Usage

You can load jemalloc on a per-process basis in your `Procfile` by prepending commands with `jemalloc.sh`:

```yaml
web: jemalloc.sh bundle exec puma -C config/puma.rb
```

You can also set `JEMALLOC_ENABLED` to `true` in your environment config to load jemalloc automatically for all processes on your project:

```bash
heroku config:set JEMALLOC_ENABLED=true
```

## Choosing a version

You can switch between jemalloc versions by setting `JEMALLOC_VERSION` in your environment. The setting will take effect the next time you build a new slug.

```bash
heroku config:set JEMALLOC_VERSION=3.6.0
git push heroku master
```

You should test each version of jemalloc with your app to find the best behavior. Each project has unique memory needs.

Available versions:

| Version | cedar-14 URL | heroku-16 URL | Notes |
| ------- | ------------ | ------------- | ----- |
| `3.6.0` | [(link)](https://dist.scryfall.com/jemalloc/cedar-14/jemalloc-3.6.0.tar.bz2) | [(link)](https://dist.scryfall.com/jemalloc/heroku-16/jemalloc-3.6.0.tar.bz2) | Usually best for Ruby projects |
| `4.5.0` | [(link)](https://dist.scryfall.com/jemalloc/cedar-14/jemalloc-4.5.0.tar.bz2) | [(link)](https://dist.scryfall.com/jemalloc/heroku-16/jemalloc-4.5.0.tar.bz2) | |
| `5.0.1` | [(link)](https://dist.scryfall.com/jemalloc/cedar-14/jemalloc-5.0.1.tar.bz2) | [(link)](https://dist.scryfall.com/jemalloc/heroku-16/jemalloc-5.0.1.tar.bz2) | Edge release (default version) |

jemalloc builds are distributed from `dist.scryfall.com`. The bundles are linked above for your inspection.

### License

Scryfall hereby releases this project to [the public domain](LICENSE.md).
