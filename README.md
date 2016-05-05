GridFTP
=======
Install GridFTP servers and clients.

Choose the mode to work by setting `gridftp_mode` to `server` (default) or `client`.

By default no anonymous users are allowed, set `gridftp_allow_anonymous: no` and change the `gridftp_anonymous_user` of your choice (defaults to `nogody`).

In order to make GridFTP usable you will need to deploy several certificates in both server and client.

For the server you will need:
- Other CA certificates you trust.
- CA certificates used to create the host certificate (needed?),
- CA certificates used to create the user certificates presented by the clients.
- Valid host certificate.

For the client you will need:
- CA certificates used to create the server's host certificate.
- User certificates

CA certificates can be deployed in two ways, using certificate repositories from known CA sources or installing locally trusted CA certificates.

You probably want to install packages repositories from known CA sources, ie IGTF, EUGridPMA, APGridPMA or TAGPMA. Each repository should be listed in `gridftp_ca_cert_repos` along with a list of packages to install. For example:
```
gridftp_ca_cert_repos:
  - name: eugridpma
    baseurl: http://dist.eugridpma.info/distribution/igtf/current/
    gpgkey: https://dist.eugridpma.info/distribution/igtf/current/GPG-KEY-EUGridPMA-RPM-3
    packages:
      - ca_policy_igtf-classic
```

Local trusted CA certificates can also be installing by listing them in `gridftp_ca_local_certs` along with their subject and signing policy. For example:
```
gridftp_ca_local_certs:
  - name:
    subject: '/O=Grid/OU=GlobusTest/CN=Globus Simple CA'
    policy: '/O=Grid/OU=GlobusTest/*'
    cert: |
      -----BEGIN CERTIFICATE-----
      ...
      -----END CERTIFICATE-----
```

You should ask your local CA representative for host (for server) and user (for client) certificates.

Requirements
------------
See `meta/main.yml`.

Role Variables
--------------
See `defaults/main.yml`.

Dependencies
------------
None.

Example Playbook
----------------
Example:
```
- hosts: gridftp-servers
  roles:
  - gridftp
```

TODO
----
- threading on clients.
- xinetd conf or gfork.
- set gridftp_instances in xinetd/gfork conf or command line.
- tune host: http://fasterdata.es.net/host-tuning/
- use udt instead of tcp?
- strped and cluster-to-cluster setup.
- chroot
- enable confidential communication (encryption)?
- get all certs in igtf or terana?
- make elixir CA the default
- create host cert/key from elixir CA

Licence
-------
Licensed under [CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

Author Information
------------------
Luis Gracia <luis.gracia@ebi.ac.uk>
