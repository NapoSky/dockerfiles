# How To Use

You probably want to bind-mount in some kind of `/etc/mailname` that actually makes sense.  Probably also worth setting `-h` to the same value.  Something like `-v /etc/mailname:/etc/mailname:ro -h "$(cat /etc/mailname)"` might be reasonable.

## sSMTP

If you want to use this with another container that has sSMTP or similar installed, here's a configuration you can adapt: (assuming your second container uses something like `--link some-exim4:smtp`)

```
Mailhub=smtp
FromLineOverride=Yes
```

Then `sendmail` in your linked container should work as expected.  You can also skip `FromLineOverride` in the second container if you want, but the alternative is sSMTP being weird about hostnames and forcing you into a box.

## Gmail

If you'd rather not relay mail directly (which is a smart thing to not want to do generally), you can trivially configure this container to relay via a Gmail account instead!  Just add `-e GMAIL_USER=youruser@yourdomain.com -e GMAIL_PASSWORD=yourpasswordhere` and the entrypoint will automatically preconfigure to relay via the Gmail account specified!

## Custom SMTP Smarthost

If you don't want to relay from a Gmail account, you can define a SMTP relay server with `-e SMTP_RELAY=yourserver.yourdomain.com`. The entrypoint will preconfigure exim4-config too.
