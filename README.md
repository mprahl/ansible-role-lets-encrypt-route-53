# lets-encrypt-route-53

This is an Ansible role that automates the generation of Let's Encrypt signed certificates with DNS challenges on
Amazon's Route 53 (AWS).

Please note that as part of this role, `openssl`, [boto](https://github.com/boto/boto), and
[pyOpenSSL](https://github.com/pyca/pyopenssl) will be installed. If you are using CentOS/Red Hat, the role will
install `pip` (requires [EPEL](https://fedoraproject.org/wiki/EPEL)) and then install `boto` and `pyOpenSSL` in a
Python virtualenv instead because the packaged version of `pyOpenSSL` is not recent enough.

If you are not using this role on Debian/Ubuntu, CentOS/Red Hat,
or FreeBSD, `openssl`, `boto`, and `pyOpenSSL` must be installed manually before using this role.

## Requirements

This role must be run by root or through sudo/become.

## Role Variables

#### Required Variables
* **ler53_aws_access_key** - the access key to an AWS user that is allowed to add records to the domain the SSL certificate is being generated for.
* **ler53_aws_secret_key** - the secret key to an AWS user that is allowed to add records to the domain the SSL certificate is being generated for.
* **ler53_route_53_domain** - the Route 53 (AWS) domain/zone the SSL certificate is being generated for.

#### Optional Variables
* **ler53_cert_common_name** - the common name for the SSL certificate being generated. This defaults to the value of `ansible_fqdn`.
* **ler53_cert_sans** - a list of DNS subject alternative names (SAN's) of the same domain as specified in `ler53_route_53_domain` to include in the CSR.
Please note that a DNS SAN for the value of `ler53_cert_common_name` is automatically added and should not be added to this list. This variable is not defined by default.
* **ler53_cert_key_usages** - a list of key usages to include in the CSR (Let's Encrypt may restrict some). This defaults to `digitalSignature` and `keyEncipherment`.
* **ler53_key_size** - the size of the private key that is paired to the certificate being generated. This defaults to `2048`.
* **ler53_cert_dir** - the path to the directory to store the private key, CSR, and certificate. This defaults to `/etc/ssl/{{ ler53_cert_common_name }}`.
* **ler53_key_file_name** - the file name of the private key that is paired to the certificate being generated. This defaults to `{{ ler53_cert_common_name }}.key`.
* **ler53_cert_file_name** - the file name of the certificate being generated. This defaults to `{{ ler53_cert_common_name }}.crt`.
* **ler53_csr_file_name** - the file name of the certificate signing request (CSR) being generated. This defaults to `{{ ler53_cert_common_name }}.csr`.
* **ler53_intermediate_download** - whether or not the Let's Encrypt intermediate CA should be downloaded. This defaults to `true`.
* **ler53_intermediate_file_name** - the file name of the intermediate CA downloaded from Let's Encrypt. This defaults to `{{ ler53_cert_common_name }}.intermediate.pem`.
* **ler53_cert_and_intermediate_file_name** - the name of the file created with the certificate and the intermediate CA concatenated together. This defaults to `{{ ler53_cert_common_name }}.pem`.
* **ler53_cert_files_mode** - the file mode/permissions to force on the private key, CSR, and certificate. This defaults to `0600`.
* **ler53_cert_files_owner** - the file owner to force on the private key, CSR, and certificate. This defaults to `root`.
* **ler53_cert_files_group** - the file group to force on the private key, CSR, and certificate. This defaults to `root`.
* **ler53_account_email** - an email to associate with your Let's Encrypt account.
Please view the [Let's Encrypt Module](https://docs.ansible.com/ansible/letsencrypt_module.html#requirements-on-host-that-executes-module) for more information.
* **ler53_account_key_size** - the size of the Let's Encrypt account key that is generated if it isn't present. This defaults to `2048`.
* **ler53_account_key_dir** - the path to the directory to store the Let's Encrypt account key. This defaults to `/etc/ssl/lets_encrypt`.
* **ler53_account_key_file_name** - the file name of the Let's Encrypt account key. This defaults to `lets_encrypt_account.key`.
* **ler53_cert_remaining_days_before_renewal** - the minimum number of days left of the current certificate being valid until it is renewed. This defaults to `10`.
* **ler53_agreement** - overwrite the default URL to the Let's Encrypt agreement. This is useful when the
Let's Encrypt agreement changes and the version of Ansible you are using has an outdated default.
* **ler53_new_cert_when_csr_changes** - delete the existing certificate when the CSR changes. This is useful
when you change the common name or SANs on your certificate and you'd like to generate a new certificate
without waiting for it to expire. This defaults to `false`.
* **ler53_cert_extended_key_usages** - Additional restrictions (e.g. client authentication, server authentication) on the allowed purposes for which the public key may be used.
* **ler53_service_handlers** - A list of dictionaries describing service handlers to run when a certificate is updated in the format of `{'name': 'httpd', 'state': 'restarted'}`.

## Example Playbook

```yaml
- name: Generate an SSL certificate for host.example.com
  hosts: host
  become: yes

  vars:
  - ler53_cert_common_name: host.example.com
  - ler53_route_53_domain: example.com
  - ler53_aws_access_key: SomeAccessKey
  - ler53_aws_secret_key: SomeSecretKey

  roles:
  - mprahl.lets-encrypt-route-53
```

## License

MIT
