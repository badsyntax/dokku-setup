# Email

Ideally you should have an external mailserver (eg [mailinabox](https://mailinabox.email/)) and each app should use that mailserver for sending mail.

If you want to send mail from the docker host (eg for deployment notifications), then follow [these instructions](https://mailinabox.email/advanced-configuration.html) to install postfix as a "Satellite system" on the dokku host.

(If you're using mailinabox, ensure the dokku user is able to send email on behalf of your mailserver login by adding new mail aliases.)
