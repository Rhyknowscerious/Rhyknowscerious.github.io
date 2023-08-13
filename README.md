# https://Rhyknowscerious.github.io

## tsds

### What is tsds and what does it do?

The app name `tsds` is an abbreviation for `Termux Simple Desired State`. It allows you to create a single git repository for your termux configuration. It allows you to only spend your time setting up your config once so that if you have to re-install Termux or install Termux on a new device, you don't have to waste a bunch of time writing your dot files, installing your favorite apps, re-making shortcuts for your Android widget for Termux, etc. If you don't load a configuration, it creates a blank one for you. The configuration sits at `~/.tsds` by default and consists of specific directories and files that `tsds` will look for.

This is inspired by enterprise grade state configuration software like ansible but instead of managing a fleet of 100s or 1000s of devices, this configures one at a time and is designed specifically for Turmux installed on non-rooted phones. It probably works on rooted phones just fine too but I haven't done any root-specific testing. 

The current release 1.0.0 will apply your configuration but it won't tear-down so any apps you configure to install with this software will need to be un-done/removed/uninstalled manually. Make sure to backup any files with names matching patterns `~/.*`, `~/.shortcuts/*`, or `$PREFIX/etc/apt/sources.list.d/tsds.list` if you don't want them to be overwritten. Overwrites should be prompted for confirmation so nothing gets deleted without your input but still back your files up before running `tsds` _just in case_.

### How to install tsds:

```sh
echo "deb [trusted=yes] https://rhyknowscerious.github.io/tsds tsds main" > $PREFIX/etc/apt/sources.list.d/tsds.list
pkg update
pkg upgrade
pkg install tsds
```

### How to use tsds:

#### Configure your Termux environment

##### Clone an existing configuration or create a blank one

You have three options: 

1. If you have your own configuration, go ahead and do that with something like `git clone https://whatever.com/whatever/your-tsds-config.git ~/.tsds`
1. If you don't have your own configuration, there's an example configuration available at https://bitbucket.org/Rhyknowscerious/tsds-config/src/master/

    ```sh
    # https command
    git clone git clone https://bitbucket.org/Rhyknowscerious/tsds-config.git ~/.tsds
    
    # or
    
    # ssh command
    git clone git@bitbucket.org:Rhyknowscerious/tsds-config.git ~/.tsds
    ```

1. If you don't have git setup at all, a blank configuration will be created for you at `~/.tsds` when you run the `tsds` command.

##### Once you have a configuration ready (by loading one or creating a blank one), it can all be exlpained here:

1. ~/.tsds/apt-repos/
    - Put files in here named `whatever.list` with information about additional repositories you'd like to add. For example, the example configuration mentioned above has `tsds.list` which enables the tsds repo.
    - When running `tsds` the app will make symbolic links to these files and put the links here: `$PREFIX/etc/apt/sources.list.d/`
1. ~/.tsds/dot-files/
    - Put files in here named `.whatever` which should be configurations like `.bashrc`,`.profile`,`.vimrc`,`.mutt/muttrc` or whatever you want.
    - When running `tsds` the app will make symbolic links to these files and put the links here: `~/`
1. ~/.tsds/favorite/ has 3 items
    1. apps-with-repos.txt
       - For each app you want to install on termux using `pkg`, add the app name on its own line. Lines starting with `#` are ignored.
    1. apps-apt-only.txt
       - This is like `apps-with-repos.txt` for apps that aren't available via `pkg`. These apps are installed with `apt`
    1. apps-without-repos/
       - This is a directory for scripts to install software that doesn't have a deb package for termux at all. Instead of developing your own deb package for new software or software that's already out there, you can just put install scripts here. The example configuration mentioned above has scripts for ansible, awscli, homebridge, and terraform.
    - When running `tsds` the app will install all the apps listed in the txt files and run all the scripts in the `apps-without-repos/` directory.
1. ~/.tsds/widget/.shortcts
   - Put scirpts in here that you'd like to see in the Android homescreen widget for Termux.
   - When running `tsds` all the scripts in `~/.tsds/widget/.shortcuts` are copied into `~/.shortcuts`. These are copied instead of linked symbolically because for some reason the widget won't read symbolic links from the tsds configuration locations.

#### When your configuration is ready apply, just execute tsds

```sh
# Execute tsds without updating pkg packages (for example you recently did a `pkg update`)
tsds

# or

# Have tsds update pkg packages before loading the config
tsds -u
```

### How to develop tsds:

Here's the source code. It's open so you can contribute or fork as you like. [Find the tsds project and development instructions here.](https://bitbucket.org/Rhyknowscerious/tsds/src/dev/src/)
