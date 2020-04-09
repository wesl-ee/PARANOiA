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

Ensure you have all the requisites satisfied; once verified, download two
(cute!) pictures which will hold your half-keys; the PARANOiA system will
scale these pictures down and convert them to common (JPEG) formats so you
need not worry about formatting or size.

Firstly create your own copy of the configuration file:

```
mkdir /etc/PARANOiA && cp -r conf-example/* /etc/PARANOiA/
```

All of the commands in this document should be run as the super-user.
Next edit `/etc/PARANOiA/paranoia.conf` to reflect your system / encryption
setup.

Next identify / download two pictures which will each store one of your two
half keys; move these into the current working directory, plug in the removable
media which you want to be your PARANOiA drive and run:

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
