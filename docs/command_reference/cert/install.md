# Install

### Description

Install will put a new Certificate on the target by creating a new CSR
request and placing the new Certificate based on the CSR on the target.

The new Certificate will be associated with a new Certificate Id on the target.

If the target has a pre existing Certificate with the given Certificate Id,
the operation should fail.

Currently `gNOIc` relies on the target to generate the CSR.

The `install` command acts as the client side of the [Cert Install RPC](https://github.com/openconfig/gnoi/blob/master/cert/cert.proto#L138) and effectively install a new certificate in 4 steps:

- Start a bi-directional gRPC stream.
- Request a CSR from the target.
- Sign the Certificate using the provided CA.
- Send the certificate to the target.

### Usage

`gnoic [global-flags] cert install [local-flags]`

### Flags

#### cert-type

The `--cert-type` flag sets the desired certificate type.

defaults to `CT_X509`

#### city

The `--city` sets the `City` part of the certificate DN (Distinguished Name)

#### common-name

The `--common-name` sets the `CommonName` part of the certificate DN (Distinguished Name)

#### country

The `--country` sets the `Country` part of the certificate DN (Distinguished Name)

#### email-id

The `--email-id` sets the `EmailID` part of the certificate DN (Distinguished Name)

#### ip-address

The `--ip-address` sets an IP address to be added to the certificate as a SAN.

#### id

The `--id` flag sets the desired certificate ID.

#### key-type

The `--key-type` flag sets the desired key type, defaults to `KT_RSA`

#### min-key-size

The `--min-key-size` flag sets the minimum desired key size, defaults to `1024`

#### org

The `--org` sets the `OrganizationName` part of the certificate DN (Distinguished Name)

#### print-csr

The `--print-csr` if set, `gNOIc` prints the CSR generated by the Target.

#### org-unit

The `--org-unit` sets the `OrganizationalUnit` part of the certificate DN (Distinguished Name)

#### state

The `--state` sets the `State` part of the certificate DN (Distinguished Name)

#### validity

The `--validity` sets the validity duration of the certificate, the expected format is Golang's duration format: 1s, 10m, 1h, 87600h.

defaults to `87600h` (10 years)

### Examples

```bash
gnoic -a 172.17.0.100:57400 --insecure -u admin -p admin \
      cert \
      --ca-cert cert.pem  --ca-key key.pem  \
      install \
      --ip-address 172.17.0.100 --common-name router1 --id cert2
```

```bash
INFO[0000] read local CA certs                          
INFO[0000] "172.17.0.100:57400" signing certificate "CN=router1" with the provided CA 
INFO[0000] "172.17.0.100:57400" installing certificate id=cert2 "CN=router1" 
INFO[0000] "172.17.0.100:57400" Install RPC successful  
```