tree# Links

>MTLS para API Gateway com CAs geridas fora da AWS: https://aws.amazon.com/blogs/compute/introducing-mutual-tls-authentication-for-amazon-api-gateway/

^261189

>MTLS para API Gateway com AWS PCA: https://aws.amazon.com/blogs/compute/automating-mutual-tls-setup-for-amazon-api-gateway/ Esta é a solução deployed com diferença que ela usa um lambda para automaticamente fazer download do certificado do AWS PCA para o bucket S3. Este processo foi feito manualmente.

^3a3d7b

# Examples
---

## Sign a certificate
---
```shell
aws acm-pca issue-certificate --certificate-authority-arn arn:aws:acm-pca:eu-central-1:793073444497:certificate-authority/186cd8e6-5df2-4a9e-a214-7f3deb0d2fcc --csr file://my_client.csr --signing-algorithm SHA256WITHRSA --validity Value=365,Type="DAYS" --profile vap-mgmt
```
```json
{
    "CertificateArn": "arn:aws:acm-pca:eu-central-1:793073444497:certificate-authority/186cd8e6-5df2-4a9e-a214-7f3deb0d2fcc/certificate/1cbe485a21fe5f6ab1fc4ab12dc3eb03"
}
```


## Get certificate
---
```bash
aws acm-pca get-certificate --certificate-authority-arn arn:aws:acm-pca:eu-central-1:793073444497:certificate-authority/186cd8e6-5df2-4a9e-a214-7f3deb0d2fcc --certificate-arn arn:aws:acm-pca:eu-central-1:793073444497:certificate-authority/186cd8e6-5df2-4a9e-a214-7f3deb0d2fcc/certificate/1cbe485a21fe5f6ab1fc4ab12dc3eb03 --profile vap-mgmt
```
```json
{
    "Certificate": "-----BEGIN CERTIFICATE-----\nMIIECjCCAvKgAwIBAgIQHL5IWiH+X2qx/EqxLcPrAzANBgkqhkiG9w0BAQsFADBB\nMQswCQYDVQQGEwJERTEXMBUGA1UECgwOVmFudGFnZSBUb3dlcnMxGTAXBgNVBAMM\nEEFQSSBHYXRld2F5IE1UTFMwHhcNMjIwOTIwMTUxOTAzWhcNMjMwOTIwMTYxOTAz\nWjBFMQswCQYDVQQGEwJQVDETMBEGA1UECAwKU29tZS1TdGF0ZTERMA8GA1UECgwI\nT3V0c2NvcGUxDjAMBgNVBAMMBU5FTElPMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A\nMIIBCgKCAQEA3I4ykctLc0Aw+u6X6vsWkfTx2jJOuZbsQDVGWZSJ4wAPDBgYN7/Q\nBPd0rURHMrqxl/Sh8KB4nwNypO4GBGl2Gr8uBr88MyFjn8A4/Lr1rmrf84ITr7Qa\ng+j+sW9Plnlq7bGzJCvvrqyhjz135OaYLGBNDWkaoAwa62o/X2CiNGrnVQ4jRvPB\nnVs6eVouzjwPNxtEO6S8x6RNCqn1nrBrhDadbqT/aByIzxZweZDnA025pKuM+PgC\nwPl8I5KuWes9SQKFZIJ4k05Kr3ZPWgAC0ufsRYhoDuaRbep5JQx9Wpe6kc4q8mtk\npH+QqZGu3IAV+c8tMFYeIqGSvEgL1awnvwIDAQABo4H5MIH2MAkGA1UdEwQCMAAw\nHwYDVR0jBBgwFoAU5hcftfpgz8lq3AegDCRst/ZGvZEwHQYDVR0OBBYEFJywvFxe\nHuMlJ31mcTqFPr1dt67gMA4GA1UdDwEB/wQEAwIFoDAdBgNVHSUEFjAUBggrBgEF\nBQcDAQYIKwYBBQUHAwIwegYDVR0fBHMwcTBvoG2ga4ZpaHR0cDovL3ZhcC1tdGxz\nLXBjYS1jcmwtYnVja2V0LnMzLmV1LWNlbnRyYWwtMS5hbWF6b25hd3MuY29tL2Ny\nbC8xODZjZDhlNi01ZGYyLTRhOWUtYTIxNC03ZjNkZWIwZDJmY2MuY3JsMA0GCSqG\nSIb3DQEBCwUAA4IBAQAKF2h8ZxyEDfZRuC6vQnkpxQ2SUeLWCVXk/8c6299m2jmP\nrNJYk0cea7PYd1ImEd9F/Gm/BiD6HF1EG0mCYdeqrJ8NdMzNB/qGBqx41nVyrjQH\ntOn0Obq+ZiO9nYoq19PUiwCPVO8Kt99UYH5aUpFg80dRcaK3I63EePlLl3x3b4uV\ngYIxpmp5WKqSU0UbsZheB3BkQiXDHbh2BWpt2tBchjsfhnuOMls4ERYQHXSkQOJ4\n9sPrAb8Lr011yb3wXzD3oIdQv11HoO9ZZkgli0Wp71fldvOP4fCll9XDnyE71Mge\n8Cnv+L2Yj5AoPG3JCKvDps+7+nLBwNzOiHnF8XVq\n-----END CERTIFICATE-----",
    "CertificateChain": "-----BEGIN CERTIFICATE-----\nMIID7DCCAtSgAwIBAgIQEn6jOc4Mb/OmLCWcrB0EgDANBgkqhkiG9w0BAQsFADA9\nMQswCQYDVQQGEwJERTEXMBUGA1UECgwOVmFudGFnZSBUb3dlcnMxFTATBgNVBAMM\nDEFQSSBQbGF0Zm9ybTAeFw0yMjA5MTMxMjQzNTZaFw0yNTA5MDcxMzQzNTZaMEEx\nCzAJBgNVBAYTAkRFMRcwFQYDVQQKDA5WYW50YWdlIFRvd2VyczEZMBcGA1UEAwwQ\nQVBJIEdhdGV3YXkgTVRMUzCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB\nAISN1lPV7ekGcyZDGJ5BiCV12/F158o2MHmBJEu6I9FWNSauzK3DmLWWx5YhR2DU\n79cLTYO17+rozluilO2AE4EQZg3P6kzYjrV4Y/14SXulsAE8AqhUeGo14ShsyInK\nfjc1HqsChPVayvjmh7coGAoANTw2w9Sj0UOYu8nj70mqyhGjoE7IMtfALNTc3rBt\nKHXdc1Z+872cuHECEDVZSKwKzhZIZ8rMriJYTl5ILGAExgreZui4lh1kFMRm/g1m\nw4bimWIA4ckbF/ZTATmTtW9VLYPCQ6ziuAZIi/iFRDbhOZTupcZZRpEHzlAQY/5q\nSbQcdwgPrABtCZSkZliC2xMCAwEAAaOB4zCB4DASBgNVHRMBAf8ECDAGAQH/AgED\nMB8GA1UdIwQYMBaAFP5ZMEp0XipZhAsr125vIBkQ+dh7MB0GA1UdDgQWBBTmFx+1\n+mDPyWrcB6AMJGy39ka9kTAOBgNVHQ8BAf8EBAMCAYYwegYDVR0fBHMwcTBvoG2g\na4ZpaHR0cDovL3ZhcC1tdGxzLXBjYS1jcmwtYnVja2V0LnMzLmV1LWNlbnRyYWwt\nMS5hbWF6b25hd3MuY29tL2NybC8xMTlhY2VhYS0yNmMzLTQ3OGYtOWMzYi0wMWFk\nMDBlNmUzYzMuY3JsMA0GCSqGSIb3DQEBCwUAA4IBAQBxaZNtbieBL3iamBMyj2Os\nIDEl5vHdrPQmqBp6V98ZHfvQMsGryg3SrsZrMe8DkAVQo30W1nCGn4l6tw0Xue+0\nj8mxkHfg3T3N1yrBCkiLh0fiaNyzQFfHTCqrd9wnbCdopr2lgvoYr4fMsBmjrWz9\nR3VdxZqkEnnoNomseL85AWYxqNQIsszXyaeuZ7J7f/8weVpvsMFVerAuLrZ6rRjl\nGw0CbsvF/idrTmD4SCNE0FCrKSEBLbD5MjVYQ27vjRINtEE9UFoxmuhGxY0vqvax\nHuHj04hBSJozs9anS56pxLS9cL5UQtHrFLWomjVYTrbzgYDauVoG+gSVxMpX6pyo\n-----END CERTIFICATE-----\n-----BEGIN CERTIFICATE-----\nMIIDRzCCAi+gAwIBAgIRALVsivbBBuvX3Tmizb0VfoMwDQYJKoZIhvcNAQELBQAw\nPTELMAkGA1UEBhMCREUxFzAVBgNVBAoMDlZhbnRhZ2UgVG93ZXJzMRUwEwYDVQQD\nDAxBUEkgUGxhdGZvcm0wHhcNMjIwOTEzMDgxOTUyWhcNMzIwOTEwMDkxOTUyWjA9\nMQswCQYDVQQGEwJERTEXMBUGA1UECgwOVmFudGFnZSBUb3dlcnMxFTATBgNVBAMM\nDEFQSSBQbGF0Zm9ybTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAK2C\nkwoUGJxwM405FEU9S5QrCgk3DPoe7U2/kQT/+GuSyy89DcrPzRJvLvoXdWtpAxTj\nc8t3qDEoKmq1NzCQ7hTA66HCj7NJZKSZqOlqOZKgQq4rkh9rSESDZqFPgQQlk4hZ\nxmf29CK6RYikDeQ+LAA3nyhWeJ7/0UoOGIFmWhc0VDAIiKpgNXai3JLLVn1qZUkZ\nPhb883R34xbzbkYYVJEWaFBCnjx47/CfsKf3id8kwSW80q0ktjpO7PGVXJsfaytJ\n7nCsoM3TN4v/2lASfzI2NXzOUsm/QxpSss1FtQmRmoFKVGmAyw2Qjz9IeTrFvzTr\nakMjmlunrLBqBitNH/cCAwEAAaNCMEAwDwYDVR0TAQH/BAUwAwEB/zAdBgNVHQ4E\nFgQU/lkwSnReKlmECyvXbm8gGRD52HswDgYDVR0PAQH/BAQDAgGGMA0GCSqGSIb3\nDQEBCwUAA4IBAQCU+7MQiBvoq5PJcLQYH+ZL61Xga/ygdCpT+a9Cj2tbtVMfwFTR\nEsMbKKxNK/lfZs4nRrHfaDPhUNOufJIQVv9BjV3BMuG9vzYhJzxuSeK4+3t0Vfzx\nK1SigKG0WWTxrfFsdy9Ktkz53DU37ZB6EnIi4iQuFV2Av8lcC0btuoHW4Hihp/LA\n2UUAAY4dKcl9EgPBtPQlyw29zaaCSoV3BI4CiIfE27i3kaJ8tTst/DDWivJIRx45\nH2AVwIEBD/EEkgV9OOI7M3qAM8njNo/sps3VVxQQU7PnPZakM6cBXB1dg9zy6aHF\neFYT7dA/3rJ2fyTo0pGkvQMfh8cwZ+RMyzV3\n-----END CERTIFICATE-----"
}
```


## Revoke Certificates
---
```bash
aws acm-pca revoke-certificate --certificate-authority-arn arn:aws:acm-pca:eu-central-1:793073444497:certificate-authority/186cd8e6-5df2-4a9e-a214-7f3deb0d2fcc --certificate-serial 1CBE485A21FE5F6AB1FC4AB12DC3EB03 --revocation-reason "UNSPECIFIED" --profile vap-mgmt
```


## openssl cert/crt/key info
---
```bash
openssl rsa -noout -modulus -in domain.key | openssl md5 - CHAVE
openssl x509 -noout -modulus -in domain.crt | openssl md5 - CERTIFICADO
openssl req -noout -modulus -in domain.csr | openssl md5 - 
```


## curl example with a certificate
---
```bash
curl --key my_client.key --cert ca.pem -X GET "https://dev.mtls.build.api.vantagetowers.com/activity/v1/milestone" -H "accept: application/json" -H "x-api-key: ke9zDGjNrx9GBJV8nfRcC51QWnlGUTKtaAQLuoLe"
```
```json
{
  "code": "ERR013",
  "reason": "Forbidden"
}
```

---
Tags:
#aws/apiGateway
#mtls
#aws/acm
#certificates