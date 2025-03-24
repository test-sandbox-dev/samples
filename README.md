# test-sandbox.dev Samples

## Service samples
Few examples of service implementation

### .Net
- **/service/dotnet/server-p12** .Net service with \*.p12 (\*.pfx) server certificate
- **/service/dotnet/server-pem** .Net service with server TLS configuration using PEM certificate and key files

When running from IDE make sure to select `https` profile. To start the sample app with the `https` profile from the command line, use the following command:

```sh
dotnet run --launch-profile https
```

## Client samples

### Adding local.test-sandbox.dev to Hosts File

Device connecting to server using certificate from test-sandbox.dev needs to define name in hosts file and access service with that name.

To add `local.test-sandbox.dev` to the hosts file on macOS or Linux, use the following command:

```sh
sudo sh -c 'echo "127.0.0.1 local.test-sandbox.dev" >> /etc/hosts'
```

To add `local.test-sandbox.dev` to the hosts file on Windows, use the following command:

```powershell
Add-Content -Path "$env:SystemRoot\System32\drivers\etc\hosts" -Value "127.0.0.1 local.test-sandbox.dev"
```

### Making test request

You can test the API call using the provided `invoke-service.http` definitions or use it as URL example to invoke same through
browser or other tool.


## Converting P12 (PFX) to PEM Files

To convert a P12 (PFX) file to PEM files with a full chain and encrypted private key, you can use the following OpenSSL commands:

1. Extract the private key from the P12 file and encrypt it:

```sh
openssl pkcs12 -in cert.p12 -nocerts -out key.pem -nodes
openssl rsa -in key.pem -out keyenc.pem -aes256
```

2. Extract the full chain (certificate and intermediate certificates) from the P12 file:

```sh
openssl pkcs12 -in cert.p12 -nokeys -out cert.pem
```

Replace `cert.p12` with the path to your P12 file. The `keyenc.pem` file will contain the encrypted private key, and the `cert.pem` file will contain the full certificate chain.

These PEM files can then be used in the `appsettings.Development.json` configuration. Note that .Net requires encrypted PEM key.

