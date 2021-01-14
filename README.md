![Sms77.io Logo](https://www.sms77.io/wp-content/uploads/2019/07/sms77-Logo-400x79.png "Sms77.io Logo")


# Official Plugin for [Nagios Core](https://www.nagios.com/products/nagios-core/)


## Installation
1. Make sure Python 2+ is installed on the system.

2. Copy [sms77.py](./sms77.py) to the Nagios plugins directory usually in /usr/local/nagios/libexec.

### Usage

Modify /usr/local/nagios/etc/objects/contacts.cfg:
```
define contact {
    #...
    pager                         +491234567890
    host_notification_commands    notify-host-by-sms
    service_notification_commands notify-service-by-sms
}
```

Append to /usr/local/nagios/etc/objects/commands.cfg:
```
# Results in a SMS like:
# RECOVERY: SMS on 127.0.0.1@localhost, State: OK, Output: 100, Date: 01-15-2021 12:30:28
define command {
 command_name notify-service-by-sms
 command_line $USER1$/sms77.py MY_SMS77_API_KEY $CONTACTPAGER$ "$NOTIFICATIONTYPE$:$SERVICEDESC$ on $HOSTADDRESS$@$HOSTNAME$, State $SERVICESTATE$, Output: $SERVICEOUTPUT$, Date: $SHORTDATETIME$" --from=Nagios
}

# Results in a SMS like:
# CUSTOM on 127.0.0.1@localhost, State: OK, Output: 100, Date: 01-15-2021 12:30:28
define command {
 command_name notify-host-by-sms
 command_line $USER1$/sms77.py MY_SMS77_API_KEY $CONTACTPAGER$ "$NOTIFICATIONTYPE$ on $HOSTADDRESS$@$HOSTNAME$, State: $HOSTSTATE$, Output: $HOSTOUTPUT$, Date: $SHORTDATETIME$" --from=Nagios
}
```

Optionally add a local service for a quick test.
Append in /usr/local/nagios/etc/objects/localhost.cfg:
```
define service {
 use                   local-service
 host_name             localhost
 service_description   SMS
 check_command         notify-host-by-sms
}
```

Available options:
```
sms77.py 
[-h] 
[--debug]
[--delay DELAY]
[--details]
[--flash]
[--foreign_id FOREIGN_ID] 
[--from FROM] 
[--json] 
[--label LABEL] 
[--no_reload] 
[--performance_tracking] 
[--return_msg_id] 
[--ttl TTL] 
[--udh UDH] 
[--unicode]
[--utf8]
api_key to text
```


#### Support
Got stuck? Feel free to [send us an email](mailto:support@sms77.io).


