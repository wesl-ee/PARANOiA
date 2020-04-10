PARANOiA Double Half-Key Cryptoystem
====================================

PARANOiA is a cryptosystem for securing LUKS devices using a "double half-key"
scheme split between the host-system and a flash drive.

Requisites
----------

The PARANOiA system expects the following:

* shred
* ImageMagick
* exif / libexif
* dm-crypt / cryptsetup

Premise
-------

The PARANOiA system complements an existing dm-crypt encryption setup,
creating a key which acts as another avenue for unlocking your partitions.
This key is composed of two half-keys, one stored on your local computer and
one stored on a removable media. The combination of these two half-keys
creates the encryption key which can unlock your partitions.

Each half-key is stored inside the EXIF data of a picture of your choice.
It's not any more secure to store the keys in this fashion; it's just a nice
touch.

The purpose of the "double half-key" is to plant one half on a removable media,
which can be removed once the machine is powered off, thereby securing the
encrypted drives against snooping without access to the other half while
creating plausible deniability in the absence of other keys or passphrases.

Installation
------------

### Portage

If you are using a portage-based distribution (e.g. Funtoo, Gentoo) then you may
install PARANOiA from my
[`raleigh-overlay`](https://github.com/wesleycoakley/raleigh-overlay) for
Poratge. Use the `layman` tool to install my overlay, then run

```
emerge sys-fs/paranoia
```

This will install the shell script and move all documentation to
`/usr/share/doc/paranoia-[version]`; to install the example configuration,
simply run:

```
mkdir /etc/PARANOiA && \
	bzcat /usr/share/doc/paranoia-[version]/paranoia.conf.bz2 > \
	/etc/PARANOiA/paranoia.conf
```

### Installation from source

Ensure you have satisfied all the dependencies. Then install this repository and
the included configuration:

```
git clone https://github.com/wesleycoakley/PARANOiA
cd PARANOiA

# As super-user
mkdir /etc/PARANOiA && cp -r conf-example/* /etc/PARANOiA/

```
Once installed, download two pictures which will hold your half-keys; the
PARANOiA system will scale these pictures down and convert them to common
(JPEG) formats so you need not worry about formatting or size.

The remaining commands in this document should be run as the super-user.
Next edit `/etc/PARANOiA/paranoia.conf` to reflect your system / encryption
setup.

Then move the two pictures you want for each half into the current working
directory, plug in the removable media which you want to be your PARANOiA
drive and run:

```
PARANOiA init picture-1 picture-2
```

This will generate a keyfile for use with the PARANOiA cryptosystem and embed
half in each of `picture-1` `picture-2`. `picture-1` will be moved to your
PARANOiA drive and `picture-2` will be stored on your local system.

```
PARANOiA add /dev/sdaX FriendlyName
```

This will add the device `/dev/sdaX` to the cryptosystem; it is assumed that the
device is already LUKS-formatted (and may be a drive that's already got data!)
This command will only add the PARANOiA key to the LUKS header of the device, no
data will be altered.

License
-------

Wesley Coakley <w@wesleycoakley.com>

All content is BSD 3-Clause (see `LICENSE`)
