# Change Log

### Unreleased
* Prerequisite packages are now installed on FreeBSD.
* Added the ability to specify SAN's for the same domain.
* Added the ability specify the key usage.

### v3.0.0
* Install the python-boto package instead of through pip.
* Removed the variables **ler53_cert_country**, **ler53_cert_state**, **ler53_cert_locality**, and **ler53_cert_organization** since these aren't used by Let's Encrypt (thanks stevenringo).
* The variable **ler53_cert_subject** was removed and now only **ler53_cert_common_name** is used.

### v2.0.0
* **ler53_intermediate_download** replaced **ler53_chain_download**.
* **ler53_intermediate_file_name** replaced **ler53_chain_file_name**.
* **ler53_cert_and_intermediate_file_name** replaced **ler53_cert_and_chain_file_name**.
* **ler53_cert_file_name** now defaults to `{{ ler53_cert_common_name }}.crt`.
* **ler53_intermediate_file_name** now defaults to `{{ ler53_cert_common_name }}.intermediate.crt`.
* **ler53_cert_and_intermediate_file_name** now defaults to `{{ ler53_cert_common_name }}.pem`.
* Ownership and permissions on **ler53_cert_dir** are now compatible with more *nix systems.

### v1.0.0
* Initial release
