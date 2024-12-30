# Use Postfix -> CURL -> ActionMailbox Ingress

Modifies a existing Postfix configuration so that it forwards specific mail addresses to a ActionMailbox ingress endpoint using a simple CURL script.

More info about this approach: https://www.stefanwienert.de/blog/2024/11/05/postfix-actionmailbox-sending-part-of-the-emails-to-postfix-by-using-aliases-curl-command/


```yaml
# playbook.yml
- hosts: all
  vars:
    action_mailboxes:
      - name: myapp_development
        email_addresses:
          # matches inputdev@example.com
          - '/bounces-.+@work-in-de.de/'
        url: 'http://work-in-de.de/rails/action_mailbox/relay/inbound_emails'
        password: '1203910240123012301230123123123123'

      - name: myapp_production
        email_addresses:
          - '/input@example.com/'
          # matches support-(*)@example.com and
          #         support+(*)@example.com
          - '/support[-+].+@example.com/'
        url: 'https://prod.app-in-the-cloud.com/rails/action_mailbox/relay/inbound_emails'
        # action_mailer.ingress_password
        password: '1203910240123012301230123123123123'
  tasks:
    - import_role:
        name: pludoni.ActionMailboxPostfix
```
