# fail2ban-slack

Yet another Slack notification action for [Fail2ban](https://www.fail2ban.org/wiki/index.php/Main_Page)

## Prerequsites

Thew following are prerequsites required to utilize this action:

- [Fail2ban](https://www.fail2ban.org/wiki/index.php/Main_Page)
- [Slack Webhook](https://api.slack.com/messaging/webhooks)
- [Curl](https://curl.se/)

## Installation

#### Step 1: Generate Slack Webhook

Follow steps outlined in Slack's [Getting started with Incoming Webhooks](https://api.slack.com/messaging/webhooks) documentaiton.

#### Step 2: Add Slack-Notify Action

```bash
# Add the slack-notify action to fail2ban
$ cp slack-notify.conf /etc/fail2ban/action.d/
```

#### Step 3: Configure Slack Environment Variables

The following environment variables are utilized by this fail2ban action:

- SLACK_WEBHOOK_URL: incoming slack webhook url to utilize for slack integration
- SLACK_CHANNEL: target slack channel for fail2ban notifications
- SLACK_USERNAME: slack user to associate to message (defaults to `fail2ban`)
- SLACK_ICON: slack icon to associate to message (defaults to `:cop:`)

These environment variables can be configured anyway you want. If fail2ban is running as a systemd servivce you can utilize the included override.conf config by following the step below:

```bash
# copy config
$ cp systemd/override.conf /etc/systemd/system/fail2ban.service.d/

# update override.conf with the required info
$ vim /etc/systemd/system/fail2ban.service.d/override.conf
```

#### Step 4: Add Slack-Notfy banaction to your jail(s)

Include the `slack-notify` banaction in your jail(s). SSHD example below:

```
$ cat /etc/fail2ban/jail.d/defaults-debian.conf

port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
banaction = iptables-multiport
            slack-notify
```

#### Step 5: Restart fail2ban

Restart the fail2ban service