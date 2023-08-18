## Summary

The message in the UI says **"We couldn't deliver this payload: x509: certificate signed by unknown authority".** Webhooks are failing due to a recently expired or updated certificate on GitHub Enterprise Server. The GHES certificate itself is valid, but GHES doesn't recognize the local certificate authority (CA) or self-signed certificate on the webhook destination host.

## Symptoms

Messages like this appear in the UI and webhooks are not deliverable.

![image](https://github.com/loujr/loujr/assets/61295275/decb924e-6e00-47cb-97ac-2d5e98eab709)

## Resolution

To resolve this error, perform the following steps:

1. Reinstall the signed certificate.

   ```shell
   ghe-ssl-ca-certificate-install -c NameOfYourRootCertificateAuthority.crt
   ```

2. Restart hookshot-go.

   ```shell
   nomad stop hookshot-go
   nomad run /etc/nomad-jobs/hookshot-go/hookshot-go.hcl
   ```

3. On the organization's webhook page, click **Redelivery** on the recently failed deliveries. The deliveries should redeliver successfully.

![Screenshot 2023-05-12 at 2 18 01 PM](https://github.com/loujr/loujr/assets/61295275/3b866565-b1ee-43ba-b9c9-c04bba991666)

If this procedure does not resolve the issue, please open a support ticket and send screenshots of all the error messages you are receiving and the IDs of the failed webhook deliveries.

## Additional Resources

- [Installing self-signed or untrusted certificate authority (CA) root certificates](https://docs.github.com/en/enterprise-server@3.8/admin/configuration/configuring-your-enterprise/troubleshooting-tls-errors#installing-self-signed-or-untrusted-certificate-authority-ca-root-certificates)
