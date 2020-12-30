## Choosing an installation media
The installers and images can be obtained on the [Tumbleweed](https://software.opensuse.org/distributions/tumbleweed) and [Leap](https://software.opensuse.org/distributions/leap) pages.

Most users are used to Live images, packing a full-blown environment where they can test 
out a Linux distribution before installing.

!!! note for Tumbleweed
    Users interested in Tumbleweed should bear in mind that, because of the way Tumbleweed is built, Tumbleweed Live images are missing a handful of tests in comparison to Tumbleweed DVD images (see below). So even if Live images can be considered reasonably safe for most users, we cannot guarantee the same quality assurance as for DVD images.
    
Our live images (~950 MBs) provide the following desktop environments:

- KDE
- GNOME
- XFCE

They each come in different flavours tailor-suited to your CPU architecture:

- x86_64 (64-bit)
- i686 (32-bit)
- aarch64 (Armv8-A "ARM" 64-bit)

The two other installation media we provide are DVD images and Network installer.

- DVD images are quite bulky (~4.5 GBs), but allow you to install an entire distribution without relying on network for retrieving components.
- Networks installers are much lighter (~150 MBs); they require a stable and hopefully fast connection to fetch components over the network.

## Authenticity & integrity checks

!!! note
    The following two steps are optional, but are considered good practice.

You perform an _integrity check_ when you want to make sure that a given file has not been tampered with. It typically involves making sure that a file's checksum matches a certain target.

You perform an _authenticity check_ when you want to make sure that a file -- typically the file that specifies the checksum target -- was obtained from a legitimate source (i.e. openSUSE). Because performing an integrity check against an inauthentic target does not make sense, you want to first do an authenticity check, and only then do an integrity check.

### Assumptions & definitions
In what follows we make the following assumptions:

1. You have downloaded an openSUSE Live, Network or DVD image from openSUSE website (hereafter: the __Image__):
    * [Tumbleweed](https://software.opensuse.org/distributions/tumbleweed)
    * [Leap](https://software.opensuse.org/distributions/leap)
2. You have obtained the __Target Checksum__ file for your image, by clicking on the _Checksum_ next to the image. 
3. You have obtained openSUSE gpg __Public Key__ either from
    ```
    $  wget https://download.opensuse.org/tumbleweed/repo/oss/gpg-pubkey-3dbdc284-53674dd4.asc
    ```
    and addded it to your keyring with
    ```
    $  gpg --import <Public Key.asc>
    ```
    or by importing it via its keyid into your gpg keyring with
    ```
    $  gpg --recv-keys 0x22C07BA534178CD02EFE22AAB88B2FD43DBDC284
    ```
4. You have obtained an openSUSE gpg __Detached Signature__ file corresponding to the image you want to perform the check upon, i.e. for KDE Live that would be
    ```
    $  wget http://download.opensuse.org/tumbleweed/iso/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso.sha256.asc
    ```

### Authenticity check
It is an instance of the command:
```
$  gpg --verify <Detached Signature.asc> <Target Checksum.iso.sha256>
```
The check passes just in case the above command outputs calls it a success.

!!! warning
    If the check fails, make sure you are honouring our [assumptions](#assumptions-definitions).

### Integrity check
It is an instance of the command:
```
$  sha256sum <Image.iso>
```
The check passes just in case the output of the above equals the checksum written in plain text in the __Target Checksum__ (see [Definitions & assumptions](#assumptions-definitions)).
