GridFTP
=======
Install GridFTP servers and clients.

Choose the mode to work by setting `gridftp_mode` to `server` (default) or `client`.

By default no anonymous users are allowed, set `gridftp_allow_anonymous: no` and change the `gridftp_anonymous_user` of your choice (defaults to `nogody`).

In order to make GridFTP usable you will need to deploy several certificates in both server and client.

For the server you will need:
- CA certificates used to create the host certificate (needed?),
- CA certificates used to create the user certificates presented by the clients.
- Other CA certificates you trust.
- Valid host certificate (and private key).

For the client you will need:
- CA certificates used to create the server's host certificate.
- Other CA certificates you trust.
- User certificates.

CA certificates can be deployed in two ways, using certificate repositories from known CA sources (on servers and clients) or installing locally trusted CA certificates (only on servers).

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

You should ask your local CA representative for host (for server) and user (for client) certificates. Once you have a valid host (trusted by your CA) and its accompanying private key install with variables `gridftp_host_cert` and `gridftp_host_key` (possible holding the host key in the vault).

This role manages globus' `grid-mapfile` directly without the use of grid-mapfile- tools. Only mappings defined in `gridftp_mapping` will get globus authorization. For example:
```
gridftp_mappings:
  - ln: vagrant
    dn: '/O=Grid/OU=GlobusTest/CN=vagrant'
```

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

- hosts: gridftp-clients
  roles:
  - gridftp
```

TODO
----
- Threading on clients
- xinetd conf or gfork
- Set gridftp_instances in xinetd/gfork conf or command line
- Tune host: http://fasterdata.es.net/host-tuning/
- Use udt instead of tcp?
- strped and cluster-to-cluster setup
- chroot
- Enable confidential communication (encryption)?
- Get all certs in igtf or terana?
- Make elixir CA the default
- Create host cert/key from custom CA
- Cron job for revocation list
- Firewall
- Manage grid-mapfile with grid-mapfile-add-entry and grid-mapfile-delete-entry tools

Licence
-------
Licensed under [CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

Author Information
------------------
Luis Gracia <luis.gracia@ebi.ac.uk>
