# https://Rhyknowscerious.github.io

How to install tsds:

```sh
echo "deb [trusted=yes] https://rhyknowscerious.github.io/tsds tsds main" > $PREFIX/etc/apt/sources.list.d/tsds.list
pkg update
pkg upgrade
pkg install tsds
```
