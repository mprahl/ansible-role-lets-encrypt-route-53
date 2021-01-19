# lets-encrypt-route-53

This is an Ansible role that automates the generation of Let's Encrypt signed certificates with DNS
challenges on Amazon's Route 53 (AWS).

Please note that as part of this role, `openssl`, [boto](https://github.com/boto/boto), and
[pyOpenSSL](https://github.com/pyca/pyopenssl) will be installed. If you are using CentOS, RHEL, or
Mac OS, the role will install `pip` and then install `boto` and `pyOpenSSL` in a Python virtualenv
instead because the packaged versions are not available or not recent enough. Please note that
[EPEL](https://fedoraproject.org/wiki/EPEL) is required on CentOS and RHEL.

If you are not using this role on Debian/Ubuntu, CentOS/Red Hat, Mac OS, or FreeBSD, `openssl`,
`boto`, and `pyOpenSSL` must be installed manually before using this role.

## Requirements

Ansible 2.7+ is required for this role. If you are using an older version of Ansible, use version
4.2.1 of this role. This role also must be run by root or through sudo/become.

## Role tags

* **install** - used for tagging the tasks that install the required dependencies.
* **openssl** - used for tagging the tasks that generate the private keys and CSR.

## Role Variables

#### Facts Set By The Role
* **ler53_cert_changed** - this is set to `True` when the certificate is created or renewed.

#### Required Variables
* **ler53_aws_access_key** - the access key to an AWS user that is allowed to add records to the
  domain the SSL certificate is being generated for.
* **ler53_aws_secret_key** - the secret key to an AWS user that is allowed to add records to the
  domain the SSL certificate is being generated for.
* **ler53_route_53_domain** - the Route 53 (AWS) domain the SSL certificate is being generated
  for. This is a required parameter, if parameter `ler53_route_53_zone_id` is not supplied.
* **ler53_route_53_zone_id** - the Route 53 (AWS) zone_id the SSL certificate is being generated
  for. This is a required parameter, if parameter `ler53_route_53_domain` is not supplied. Use
  this parameter if you don't have the AWS rights to perform route53:ListHostedZones.

#### Optional Variables
* **ler53_cert_common_name** - the common name for the SSL certificate being generated. This
  defaults to the value of `ansible_fqdn`.
* **ler53_cert_sans** - a list of DNS subject alternative names (SAN's) of the same domain as
  specified in `ler53_route_53_domain` to include in the CSR. Please note that a DNS SAN for the
  value of `ler53_cert_common_name` is automatically added and should not be added to this list.
  This variable is not defined by default.
* **ler53_cert_key_usages** - a list of key usages to include in the CSR (Let's Encrypt may
  restrict some). This defaults to `digitalSignature` and `keyEncipherment`.
* **ler53_key_size** - the size of the private key that is paired to the certificate being
  generated. This defaults to `2048`.
* **ler53_cert_dir** - the path to the directory to store the private key, CSR, and certificate.
  This defaults to `/etc/ssl/{{ ler53_cert_common_name }}`.
* **ler53_key_file_name** - the file name of the private key that is paired to the certificate
  being generated. This defaults to `{{ ler53_cert_common_name }}.key`.
* **ler53_cert_file_name** - the file name of the certificate being generated. This defaults to
  `{{ ler53_cert_common_name }}.crt`.
* **ler53_csr_file_name** - the file name of the certificate signing request (CSR) being generated.
  This defaults to `{{ ler53_cert_common_name }}.csr`.
* **ler53_intermediate_download** - whether or not the Let's Encrypt intermediate CA should be
  downloaded. This defaults to `true`.
* **ler53_intermediate_file_name** - the file name of the intermediate CA downloaded from Let's
  Encrypt. This defaults to `{{ ler53_cert_common_name }}.intermediate.pem`.
* **ler53_cert_and_intermediate_file_name** - the name of the file created with the certificate and
  the intermediate CA concatenated together. This defaults to `{{ ler53_cert_common_name }}.pem`.
* **ler53_cert_files_mode** - the file mode/permissions to force on the private key, CSR, and
  certificate. This defaults to `0600`.
* **ler53_cert_files_owner** - the file owner to force on the private key, CSR, and certificate.
  This defaults to `root`.
* **ler53_cert_files_group** - the file group to force on the private key, CSR, and certificate.
  This defaults to `root`.
* **ler53_account_email** - an email to associate with your Let's Encrypt account. Please view the
  [Let's Encrypt Module](https://docs.ansible.com/ansible/letsencrypt_module.html#requirements-on-host-that-executes-module)
  for more information.
* **ler53_account_key_size** - the size of the Let's Encrypt account key that is generated if it
  isn't present. This defaults to `2048`.
* **ler53_account_key_dir** - the path to the directory to store the Let's Encrypt account key. This
  defaults to `/etc/ssl/lets_encrypt`.
* **ler53_account_key_file_name** - the file name of the Let's Encrypt account key. This defaults
  to `lets_encrypt_account.key`.
* **ler53_cert_remaining_days_before_renewal** - the minimum number of days left of the current
  certificate being valid until it is renewed. This defaults to `10`.
* **ler53_new_cert_when_csr_changes** - delete the existing certificate when the CSR changes. This
  is useful when you change the common name or SANs on your certificate and you'd like to generate
  a new certificate without waiting for it to expire. This defaults to `false`.
* **ler53_cert_extended_key_usages** - Additional restrictions (e.g. client authentication, server
  authentication) on the allowed purposes for which the public key may be used.
* **ler53_service_handlers** - A list of dictionaries describing service handlers to run when a
  certificate is updated in the format of `{'name': 'httpd', 'state': 'restarted'}`.
* **ler53_acme_directory** - The ACME directory to use. This defaults to
  `https://acme-v02.api.letsencrypt.org/directory`. This can be useful to override if you'd like to
  test this role against the stage Let's Encrypt instance.

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
