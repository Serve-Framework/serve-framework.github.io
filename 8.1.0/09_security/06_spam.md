# SPAM

- [Access](#access)
- [Configuration](#configuration)
- [Usage](#usage)
- [Point System](#point-system)

--------------------------------------------------------

Serve's comes with built in SPAM protection for both the framework and the CMS. The CMS uses Serve's SPAM service to moderate post comments but you can you also use it directly for your own needs.

--------------------------------------------------------

### Access

You can access the `Spam` service directly via the IoC container:
```php
$spam = $serve->Spam;
```

--------------------------------------------------------

### Configuration

The SPAM service can be configured in the `app/configurations/spam.php` file.

--------------------------------------------------------

### Usage

The `isSPAM` method returns `TRUE` if text is SPAM or `FALSE` if not:
```php
if ($spam->isSpam('foooofdscz'))
{
    # That text is spam
}
```

The `rating` method returns a spam rating on text (<= 0 is SPAM anything else is not):

```php
if ($spam->rating('foooofdscz') <= 0)
{
    # That text is spam
}
```

The `isIpWhiteListed` method returns `TRUE` if an IP address is whitelisted or `FALSE` if not:
```php
if ($spam->isIpWhiteListed('192.168.1.1'))
{
    # IP is ok
}
```

The `isIpBlacklisted` method returns `TRUE` if an IP address is blacklisted or `FALSE` if not:
```php
if ($spam->isIpBlacklisted('192.168.1.1'))
{
    # IP blacklisted
}
```

The `blacklistIpAddress` method adds an IP address to the blacklist:
```php
$spam->blacklistIpAddress('192.168.1.1');
```

The `unBlacklistIpAddress` method removes an IP address from the blacklist:
```php
$spam->unBlacklistIpAddress('192.168.1.1');
```

The `whitelistIpAddress` method adds an IP address to the whitelist:
```php
$spam->whitelistIpAddress('192.168.1.1');
```

The `unWhitelistIpAddress` method removes an ip from the whitelist:

```php
$spam->unWhitelistIpAddress('192.168.1.1');
```
--------------------------------------------------------

### Point System

The SPAM service uses a point system to determine if a piece of text is considered spam. The `SpamProtector` runs through a validation process, for everything in a string that it likes, the string gets a point. For everything it doesn't like, it looses a point (or two, or three). At the end of the process, If the string has a score of `1` or higher, it's considered valid. If it gets a `0`, it should be moderated manually. If it's below `0`, it's considered spam.

The spam system uses the following rules:

| Condition                                                                                            | Action               | Points                  |
|------------------------------------------------------------------------------------------------------|----------------------|-------------------------|
| The IP address is in the ip whitelist.                                                               | Comment is approved. | -                       |
| The IP address is in the ip blacklist.                                                               | Comment is spam.     | -                       |
| The comment contains a blacklisted construct (e.g starts with "You have made such great points..."). | Comment is spam.     | -                       |
| The comment contains blacklisted code (e.g "href='javascript").                                      | Comment is spam.     | -                       |
| The comment contains blacklisted url shorteners (e.g "clck.ru").                                     | Comment is spam.     | -                       |
| The commenter's name contains gibberish (e.g "gfasfa").                                              | Comment is spam.     | -                       |
| The commenter's email address contains gibberish (e.g "gfasfa") or is invalid.                       | Comment is spam.     | -                       |
| The comment contains gibberish (e.g "gfasfa").                                                       | Comment is spam.     | -                       |
| The comment contains more than 2 links.                                                              | -                    | `-1` for each link.     |
| The comment contains less than 2 links.                                                              | -                    | `+2`                    |
| The comment is less than 20 characters long.                                                         | -                    | `-1`                    |
| The comment is more than 20 characters long.                                                         | -                    | `+2`                    |
| The comment contains greylisted words (e.g "free").                                                  | -                    | `-1` For for each term. |
| The comment contains greylisted domain links (e.g ".cn" ".xxx").                                     | -                    | `-1` For for each link. |
| The comment email address is from a greylisted domain (e.g ".cn" ".xxx").                            | -                    | `-1`                    |
| The comment contains a greylisted construct (e.g starts with "amazing...").                          | -                    | `-10`                   |
