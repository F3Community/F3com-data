# Bcrypt
The Bcrypt plugin is a secure and lightweight password hashing library.

---

Namespace: `\` <br>
File location: `lib/bcrypt.php`

<div class="alert alert-info">
<b>Notice:</b> This plugin uses BLOWFISH to hash your passwords, which needs at least PHP version 5.3.7. Keep in mind to implement alternative hashing methods, if you aim to support lower versions as well (F3 itself just needs PHP 5.3.3).
</div>


### Instantiation

```php
$crypt = \Bcrypt::instance();
```

### hash
**Generate bcrypt hash of string**

```php
string|FALSE hash ( string $pw [, bool $salt = 0 [, int $cost = 10 ]] )
```

The `$cost` parameter triggers the iteration count for the underlying Blowfish-based hashing algorithmeter and must be in range `04-31`.

### needs_rehash
**Check if password is still strong enough**

```php
bool needs_rehash ( string $hash [, int $cost = 10 ] )
```
If you decide to move to stronger passwords, you can check if the password hash will meet that `$cost` requirement. In case it's too weak, you could inform the user to choose a stronger one.

### verify
**Verify password against hash using timing attack resistant approach**

```php
bool verify ( string $pw, string $hash )
```
