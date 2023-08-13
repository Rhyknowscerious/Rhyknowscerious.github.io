# https://Rhyknowscerious.github.io

How to install tsds:

```sh
echo "deb [trusted=yes] https://rhyknowscerious.github.io/tsds tsds main" > $PREFIX/etc/apt/sources.list.d/tsds.list
pkg update
pkg upgrade
pkg install tsds
```

How to use tsds:

Edit files and create scripts in `~/.tsds` then apply the configuration with the `tsds` command.

How to develop tsds:

Here's the source code. It's open so you can contribute or fork as you like. [tsds](https://bitbucket.org/Rhyknowscerious/tsds/src/dev/src/)
