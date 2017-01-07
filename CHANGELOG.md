# Change Log

### v3.0.0
* Install the python-boto package instead of pip.

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
