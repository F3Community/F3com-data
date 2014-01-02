# SMTP : SMTP plug-in

The SMTP class is a SMTP plug-in to prepare e-mail messages (headers & attachments) and send them through a socket connection.

Namespace: `\` <br>
File location: `lib/smtp.php`

---

## Instantiation

**Return class instance**

```php
$smtp = new SMTP ( $host, $port, $scheme, $user, $pw );

```
Please refer to the [__construct](smtp#&#95;&#95;construct) method for details.

The SMTP class extends the [Magic](magic) class.

## Methods

### set

**Bind value to e-mail header**

``` php
string set ( string $key, string $val ) 
```

This function allows you to bind a value to an e-mail header. 
(Returns the `$val` value.)

Example:

``` php
echo $smtp->set('Errors-to', '<bluehole@fatfreeframework.com>');
echo $smtp->set('Sender', '<smtp-plug-in@fatfreeframework.com');
echo $smtp->set('Subject', 'Sent with the F3 SMTP plug-in');
```

### get

**Return value of e-mail header**

``` php
string|NULL get ( string $key ) 
```

This function allows you to return the value of an e-mail header. 

Example:

``` php
echo $smtp->get('From'); // displays e.g. 'J. W. von Goethe <jwgoethe@famousauthors.org>'
```

### exists

**Return TRUE if header exists**

``` php
bool exists ( string $key ) 
```

This function returns `TRUE` if a header exists as per the function `set()` described above. 

Example:

``` php
$has_date_header = $smtp->exists('Date'); // returns TRUE
```

### clear

**Remove header**

``` php
NULL clear ( string $key ) 
```

This function allows you to remove a header. 

Example:

``` php
$smtp->clear('In-Reply-To');

```

### attach

**Add e-mail attachment**

``` php
NULL attach ( $filename ) 
```

This function allows you to add an e-mail attachment given by its filename.

`attach` checks whether the filename is a regular file or not (as per the PHP function [is_file](http://www.php.net/is_file "php.net :: ")), otherwise an `user_error` is raised.

Example:

``` php
$smtp->attach( './files/pdf/'.$pdf );
$smtp->attach( './pictures/'.$screenshot ); // you can attach as many attachments you need to the same e-mail
```

### send

**Transmit message**

``` php
bool send ( string $message ) 
```

This function allows you to transmit a message. `send` opens a socket connection using the settings provided when instanciating the class. (see [__construct](smtp#&#95;&#95;construct) below for details). 

The `'From'`, `'To'` & `'Subject'` headers are mandatory, and the `$message` as well; otherwise an `user_error` is raised. 

Returns `TRUE` on success or `FALSE` when 

+ Failed to establish a socket connection with the host. 
+ SSL is unavailable on the server while the SMTP object has been instanciated with `$scheme` == 'ssl'. 

Example:

``` php
$smtp->send($message); // returns TRUE or FALSE
```

### log

**Return client-server conversation history**

``` php
string log (  ) 
```

This function allows you to retrieve the client-server conversation history under the form of a command-reply log.

Example:

``` php
echo '<pre>'.$smtp->log().'</pre>';
```
```
// Outputs:
250-8BITMIME
250-AUTH LOGIN PLAIN XOAUTH XOAUTH2 PLAIN-CLIENTTOKEN
250 CHUNKING
AUTH LOGIN
235 2.7.0 Accepted
MAIL FROM: 
(...)
QUIT
502
```

### __construct

**Instantiate class**

``` php
__construct ( string $host, int $port, string $scheme, string $user, string $pw ) 
```

The constructor allows you to instantiate the class and specify the settings that will be used by the `send` function. 

+ `$host` & `$port` of the SMTP server you want to use to send your e-mail messages.
+ `$scheme` allows you to use a SSL connection, provided you have the [openssl extension](http://www.php.net/openssl "php.net :: OpenSSL") loaded on the server.
+ `$scheme` allows you to use a TLS connection. The encryption will be based on the `STREAM_CRYPTO_METHOD_TLS_CLIENT` method as per the PHP function [stream_socket_enable_crypto](http://www.php.net/manual/en/function.stream-socket-enable-crypto.php "php.net :: stream_socket_enable_crypto").
+ `$user` & `$pw` are used to authenticate with the `AUTH LOGIN` SMTP command.

Example:

``` php
$smtp_ssl = new SMTP ( $host, $port, 'ssl', $user, $pw );
$smtp_tls = new SMTP ( $host, $port, 'tls', $user, $pw );

```

### fixheader

**Fix header**

``` php
protected string fixheader ( string $key ) 
```

This function allows to fix a header 

This _protected_ method is used internally by the `get`, `set`, `exists` & `clear` methods to check and ensure a given header value is well-formed (basically it removes forbidden characters).

### dialog

**Send SMTP command and record server response**

``` php
protected dialog ( [ string $cmd = NULL [, bool $log = NULL ]] ) 
```

This _protected_ method is used internally by the `send` method and allows to send SMTP command and record server response. 
