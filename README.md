<p align="center">
  <img src="https://www.seven.io/wp-content/uploads/Logo.svg" width="250" alt="seven logo" />
</p>

<h1 align="center">seven SMS for Nagios Core</h1>

<p align="center">
  Plugin for <a href="https://www.nagios.com/products/nagios-core/">Nagios Core</a> that sends SMS notifications via the seven gateway.
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-teal.svg" alt="MIT License" /></a>
  <img src="https://img.shields.io/badge/Nagios-Core-orange" alt="Nagios Core" />
  <img src="https://img.shields.io/badge/Python-2%20|%203-yellow" alt="Python 2 | 3" />
</p>

---

## Features

- **Native Nagios Plugin** - Drop-in `seven.py` script for `notify-host-by-sms` and `notify-service-by-sms` commands
- **Custom Sender** - Override the displayed sender via `--from`
- **Advanced SMS Options** - Flash, TTL, performance tracking, foreign ID, label

## Prerequisites

- Nagios Core
- Python 2 or 3
- A [seven account](https://www.seven.io/) with API key ([How to get your API key](https://help.seven.io/en/developer/where-do-i-find-my-api-key))

## Installation

Copy `seven.py` into the Nagios plugins directory (typically `/usr/local/nagios/libexec`).

## Configuration

### 1. Wire the contact

Edit `/usr/local/nagios/etc/objects/contacts.cfg`:

```cfg
define contact {
    # ...
    pager                         +491234567890
    host_notification_commands    notify-host-by-sms
    service_notification_commands notify-service-by-sms
}
```

### 2. Define commands

Append to `/usr/local/nagios/etc/objects/commands.cfg`:

```cfg
define command {
    command_name notify-service-by-sms
    command_line python $USER1$/seven.py MY_SEVEN_API_KEY $CONTACTPAGER$ "$NOTIFICATIONTYPE$:$SERVICEDESC$ on $HOSTADDRESS$@$HOSTNAME$, State $SERVICESTATE$, Output: $SERVICEOUTPUT$, Date: $SHORTDATETIME$" --from=Nagios
}

define command {
    command_name notify-host-by-sms
    command_line python $USER1$/seven.py MY_SEVEN_API_KEY $CONTACTPAGER$ "$NOTIFICATIONTYPE$ on $HOSTADDRESS$@$HOSTNAME$, State: $HOSTSTATE$, Output: $HOSTOUTPUT$, Date: $SHORTDATETIME$" --from=Nagios
}
```

### 3. (optional) Add a quick-test service

Append to `/usr/local/nagios/etc/objects/localhost.cfg`:

```cfg
define service {
    use                  local-service
    host_name            localhost
    service_description  SMS
    check_command        notify-host-by-sms
}
```

## Usage

```text
seven.py [-h] [--delay DELAY] [--flash] [--foreign_id FOREIGN_ID]
         [--from FROM] [--label LABEL] [--performance_tracking]
         [--ttl TTL] [--udh UDH]
         api_key to text
```

## Support

Need help? Feel free to [contact us](https://www.seven.io/en/company/contact/) or [open an issue](https://github.com/seven-io/nagios/issues).

## License

[MIT](LICENSE)
