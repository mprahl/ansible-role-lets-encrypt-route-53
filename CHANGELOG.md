# Change Log

### v5.2.0 and Newer

See the change log in the GitHub release notes.

### v5.1.2

- Support newer versions of the `openssl_privatekey_info` Ansible module (#43)

### v5.1.1

- Replace expired Let's Encrypt intermediate certificate (#42)

### v5.1.0

- Determine the correct FreeBSD Python packages automatically

### v5.0.0

- Add support for RHEL 8
- Use Python 3.7 packages for FreeBSD
- Add support for the zone ID `route53` option for accounts with limited Route
  53 permissions
- Add the ability to change the intermediate CA to download
- Fix a bug in the Let's Encrypt challenge verification step

### v4.7.0

- Use the new R3 intermediate CA
  ([#33](https://github.com/mprahl/ansible-role-lets-encrypt-route-53/issues/33))

### v4.6.1

- Add the "install" tag to the "use the created virtualenv" task

### v4.6.0

- Add support for MacOS

### v4.5.3

- Pin the version of setuptools in the RHEL/CentOS virtualenv to continue
  supporting Python 2

### v4.5.2

- Allow calling the role in a loop by setting explicit variable names for the
  loops in the role

### v4.5.1

- Fix the verification step for wildcard common names

### v4.5.0

- Add the `install` tag
- Add the `openssl` tag
- Consolidate the tasks that were used to set permissions into the existing
  OpenSSL tasks
- Verify the consistent of the private keys and CSR

### v4.4.1

- Add Python 3 support for Debian based systems

### v4.4.0

- Switch ACME protocol version to v2 as v1 will be deprecated in November 2019
  (https://community.letsencrypt.org/t/end-of-life-plan-for-acmev1/88430)

### v4.3.0

- Set the `ler53_cert_changed` fact when the certificate is created or renewed
- Notify `handle services` on validation instead of initiation
- Fix deprecated when statements

### v4.2.2

- Require Ansible 2.7
- Fix package installer deprecation warnings in Ansible 2.7

### v4.2.1

- Use the acme_certificate module instead of the deprecated letsencrypt module

### v4.2.0

- Add the ability to run service handlers when a certificate is updated with
  **ler53_service_handlers**.

### v4.1.0

- Add the ability to specify extended key usage on CSRs (thanks @calebtonn).

### v4.0.0

- Allow specifying a custom URL to the Let's Encrypt agreement.
- Migrate to the Ansible OpenSSL modules for idempotence (now installs
  pyOpenSSL).
- Use Python virtualenvs when installing boto and pyOpenSSL on RHEL/CentOS.
- Add the ability to recreate a cert when the CSR changes.

### v3.1.0

- Install prerequisite packages on FreeBSD.
- Add the ability to specify SAN's for the same domain.
- Add the ability specify the key usage.

### v3.0.0

- Install the python-boto package instead of through pip.
- Remove the variables **ler53_cert_country**, **ler53_cert_state**,
  **ler53_cert_locality**, and **ler53_cert_organization** since these aren't
  used by Let's Encrypt (thanks @stevenringo).
- Remove the variable **ler53_cert_subject** and now only use
  **ler53_cert_common_name**.

### v2.0.0

- Replace **ler53_intermediate_download** with **ler53_chain_download**.
- Replace **ler53_intermediate_file_name** with **ler53_chain_file_name**.
- Replace **ler53_cert_and_intermediate_file_name** with
  **ler53_cert_and_chain_file_name**.
- Default **ler53_cert_file_name** to `{{ ler53_cert_common_name }}.crt`.
- Default **ler53_intermediate_file_name** to
  `{{ ler53_cert_common_name }}.intermediate.crt`.
- Default **ler53_cert_and_intermediate_file_name** to
  `{{ ler53_cert_common_name }}.pem`.
- Make ownership and permissions on **ler53_cert_dir** compatible with more
  \*nix systems.

### v1.0.0

- Initial release.
