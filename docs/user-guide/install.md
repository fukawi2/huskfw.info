# Installation

Recommended installation is via git. This makes it easy to update to the latest version in the future.

After installing git, youcan fetch the latest code from GitHub and install:

## Initial Installation

```
cd /usr/local/src/
git clone git://github.com/fukawi2/husk.git
cd husk
make test
make install
```

## Initial Configuration

```
cp /usr/local/share/doc/husk/doc/rules.conf.standalone /etc/husk/rules.conf
```

1. Edit `husk.conf`
1. Edit `interfaces.conf`
1. Edit `rules.conf`
1. Test compile: `husk`
1. Compile and activate ruleset: `fire`

## Updates

```
cd /usr/local/src/husk/
git pull
make fallback    (Optional: creates a backup of current files)
make test
make install
```
