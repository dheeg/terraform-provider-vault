---
layout: "vault"
page_title: "Vault: vault_pki_secret_backend_issuers data source"
sidebar_current: "docs-vault-datasource-pki-secret-backend-issuers"
description: |-
  Lists all issuers under a particular mount.
---

# vault\_pki\_secret\_backend\_issuers

Lists all issuers under a particular mount.

~> **Important** All data retrieved from Vault will be
written in cleartext to state file generated by Terraform, will appear in
the console output when Terraform runs, and may be included in plan files
if secrets are interpolated into any resource attributes.
Protect these artifacts accordingly. See
[the main provider documentation](../index.html)
for more details.

## Example Usage

```hcl
resource "vault_mount" "pki" {
  path        = "pki"
  type        = "pki"
  description = "PKI secret engine mount"
}

resource "vault_pki_secret_backend_root_cert" "root" {
  backend     = vault_mount.pki.path
  type        = "internal"
  common_name = "example"
  ttl         = "86400"
  issuer_name = "example"
}

data "vault_pki_secret_backend_issuers" "test" {
  backend     = vault_pki_secret_backend_root_cert.root.backend
}
```

## Argument Reference

The following arguments are supported:

* `namespace` - (Optional) The namespace of the target resource.
  The value should not contain leading or trailing forward slashes.
  The `namespace` is always relative to the provider's configured [namespace](/docs/providers/vault#namespace).
  *Available only for Vault Enterprise*.

* `backend` - (Required) The path to the PKI secret backend to
  read the issuers from, with no leading or trailing `/`s.

## Attributes Reference

In addition to the arguments above, the following attributes are exported:

* `keys` - Keys used by issuers under the backend path.

* `key_info` - Map of issuer strings read from Vault.

* `key_info_json` - JSON-encoded issuer data read from Vault.
