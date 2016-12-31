# lets-encrypt-route-53

This is an Ansible role that automates the generation of Let's Encrypt signed certificates with DNS challenges on Amazon's Route 53 (AWS).
Please note that as part of this role, `openssl` will be installed as well as the latest [boto](https://github.com/boto/boto),
which is a Python module that is used to interact with AWS.

## Requirements

This role must be run by root or through sudo/become.

## Role Variables

#### Required Variables
* **ler53_aws_access_key** - the access key to an AWS user that is allowed to add records to the domain the SSL certificate is being generated for.
* **ler53_aws_secret_key** - the secret key to an AWS user that is allowed to add records to the domain the SSL certificate is being generated for.
* **ler53_route_53_domain** - the Route 53 (AWS) domain/zone the SSL certificate is being generated for.

#### Optional Variables
* **ler53_cert_common_name** - the common name for the SSL certificate being generated. This defaults to the value of `ansible_fqdn`.
* **ler53_cert_country** - the country specified in the certificate's subject. This defaults to `US`.
* **ler53_cert_state** - the state specified in the certificate's subject. This defaults to `MA`.
* **ler53_cert_locality** - the locality/city specified in the certificate's subject. This defaults to `Boston`.
* **ler53_cert_organization** - the organization specified in the certificate's subject. This defaults to `NA`.
* **ler53_cert_subject** - the subject on the generated certificate. When this is not specified, it is properly constructed with the values of
`ler53_cert_common_name`, `ler53_cert_country`, `ler53_cert_state`, `ler53_cert_locality`, and `ler53_cert_organization`.
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
