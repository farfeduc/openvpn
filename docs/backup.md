# Backing Up Configuration and Certificates

## Security

The resulting archive from this back-up contains all credential to impersonate the server at a minimum.  If the client private keys are generated using the EasyRSA utility then it also contains the client certificates that could be used to impersonate said clients.  Most importantly, if the certificate authority key is in this archive (as it is given the quick start directions), then a adversary could generate certificates at will.

I'd recommend encrypting the archive with something strong (e.g. gpg or openssl + AES).  For the paranoid keep backup offline.  For the truly paranoid users, never keep any keys (i.e. client and certificate authority) in the docker container to begin with :).


TL;DR Protect the resulting archive file, by ensure there is very limited access to it.

## Backup to Archive

    docker run --volumes-from $OVPN_DATA --rm busybox tar -cvf - -C /etc openvpn | xz > openvpn-backup.tar.xz

## Retore to New Image

    xzcat openvpn-backup.tar.xz | docker run --name $OVPN_DATA -v /etc/openvpn -i busybox tar -xvf - -C /etc0
