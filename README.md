# namesilo-php-sdk
A standalone, fully documented, bugless, fast and super easy to use PHP SDK to register domain names.

<a name="top"></a>
#### Quick start

Set `$ns_key` to your api key provided by namesilo and then include namesilo.php in your application.

#### Setting API key
In namesilo.php file set your api key:
```php
$ns_key = 'your api key goes here';
```
> To receive a sandbox API key for testing, please contact namesilo.

#### Production vs Development
For development edit namesilo.php and set
```php
$ns_url = 'http://sandbox.namesilo.com/api/';
```
For production set
```php
$ns_url = 'https://www.namesilo.com/api/'; 
```

#### Debugging 
To enable debugging edit namesilo.php and set $ns_debug to true.
```php
$ns_debug = true;
```
Once enabled, all `ns_*`  functions   `print_r()`  their request and return value.

> never turn debugging on in  production.

#### Usage example
Retrieve the lock status for exmaple.com

```php
$lock_status = ns_lock_status('example.com');

if($lock_status)
    echo 'domain is lock';
else
    echo 'domain is unlock';
```

#### Usage example 2
Locking example.com:

```php
if(ns_domain_lock('example.com')){
   echo 'Successfully locked the domain.';
}else{
   echo 'The reason it failed: ' . $ns_error;
}
```
<blockquote>
 ns_domain_lock() retruns true on success and false on failure.
</blockquote>
<blockquote>
$ns_error is a global variable set by ns_domain_lock()
</blockquote>
<blockquote>
	if you call ns_domain_lock() inside another function make sure to define $ns_error as a global variable <code>global $ns_error</code>
</blockquote>

#### List of functions

- [ns_create_contact()](#ns_create_contact):  create a new contact id for later use.
- [ns_delete_contact()](#ns_delete_contact): useful for deleting a contact if registration fails
- [ns_register_domain_by_contact_id()](#ns_register_domain_by_contact_id()]
- [ns_register_domain()](#ns_register_domain): create a new contact id and register domain at once (not recommended)
- [ns_update_nameservers()](#ns_update_nameservers) 
- [ns_update_contact_by_domain()](#ns_update_contact_by_domain)
- [ns_add_dns_record()](#ns_add_dns_record)
- [ns_get_dns_records()](#ns_get_dns_records): returns an array of dns records
- [ns_delete_dns_record()](#ns_delete_dns_record)


<a name="ns_create_contact"/>
#### ns_create_contact()
Use this function to create a new contact-id and then use the contact-id retuned by this function to associate it with a new domain registration.

> returns created contact id on success and false on failure.

```php
$contact_id = ns_create_contact(
      $fn, // first name
      $ln, // last name
      $ad, // address
      $cy, // city
      $st, // state
      $zp, // zip
      $ct, // country
      $em, // email
      $ph  // phone
);
if($contact_id){
    echo 'new contact id: ' . $contact_id;
}else{
    echo 'could not create contact id because: ' . $ns_error;
}
```


<a name="ns_register_domain_by_contact_id"/>
#### ns_register_domain_by_contact_id()
> returns true on success and false on failure.

```php
$result = ns_register_domain_by_contact_id($domain,$contact_id,$years=1);

if($result){
    echo 'success';
}else{
    echo 'fail reason: ' . $ns_error;
}
```

> This function will automatically attemp to delete the contact if registration fails. 


<a name="ns_register_domain"/>
#### ns_register_domain()
Create a new contact-id and register a domain for it at once. This method is not recommended.

> returns created contact id on success and false on failure.

```php
$result = ns_register_domain(
      $domain, // example.com
      $fn, // first name
      $ln, // last name
      $ad, // address
      $cy, // city
      $st, // state
      $zp, // zip
      $ct, // country
      $em, // email
      $ph,  // phone
      $years = 1
);
if($contact_id){
    echo 'success';
}else{
    echo 'failure reason: ' . $ns_error;
}
```




<a name="ns_update_contact_by_domain"/>
#### ns_update_contact_by_domain()
> returns true on success and false failure.

```php
$result = ns_update_contact_by_domain(
      $domain, // example.com
      $fn, // first name
      $ln, // last name
      $ad, // address
      $cy, // city
      $st, // state
      $zp, // zip
      $ct, // country
      $em, // email
      $ph  // phone
);
if($contact_id){
    echo 'Success';
}else{
    echo 'failure reason: ' . $ns_error;
}
```


<a name="ns_update_nameservers"/>
#### ns_update_nameservers()
Change the NameServers associated with one of your domains.
> returns true on success and false on success and false on failure.

```php
$result = ns_update_nameservers('example.com','ns1.namesilo.com','ns2.namesilo.com');

if($result){
    echo 'success';
}else{
    echo 'fail reason: ' . $ns_error;
}
```

<a name="ns_delete_contact"/>
#### ns_delete_contact()
> returns true on success and false on failure.

```php
$result = ns_delete_contact($contact_id);

if($result){
    echo 'success';
}else{
    echo 'fail reason: ' . $ns_error;
}
```

<a name="ns_add_dns_record"/>
#### ns_add_dns_record()
> returns true on success and false on failure.

```php
$result = ns_add_dns_record($domain,$type,$host,$value,$distance='',$ttl='');
if($result){
    echo 'success';
}else{
    echo 'fail reason: ' . $ns_error;
}
```
<blockquote>
$type possible values are "A", "AAAA", "CNAME", "MX" and "TXT"
</blockquote>
<blockquote>
$host  The hostname for the new record (there is no need to include the ".DOMAIN")
</blockquote>
<blockquote>
$value 
	<ul>
		<li>A - The IPV4 Address</li>
		<li>AAAA - The IPV6 Address</li>
		<li>CNAME - The Target Hostname</li>
		<li>MX - The Target Hostname</li>
		<li>TXT - The Text</li>				
	</ul>
</blockquote>
<blockquote>
$distance: Only used for MX (default is 10 if not provided)
</blockquote>
<blockquote>
$ttl: The TTL for the new record (default is 7207 if not provided)
</blockquote>


<a name="ns_get_dns_records"/>
#### ns_get_dns_records()
> returns an array of records on success and false on failure.

```php
$result = ns_get_dns_records($domain);
if($result){
    echo 'success';
}else{
    echo 'fail reason: ' . $ns_error;
}
```

Sample `print_r()` of successfull return value:

```
Array
(
    [0] => Array
        (
            [record_id] => a8c4251d3c3d114d70d48dcaf0288257
            [type] => A
            [host] => sunsed-test15.com
            [value] => 173.255.255.106
            [ttl] => 7200
            [distance] => 0
        )

)
```


<a name="ns_delete_dns_record"/>
#### ns_delete_dns_record()

> returns true on success and false on failure.

$result = ns_delete_dns_record($domain,$record_id);
if($result){
    echo 'success';
}else{
    echo 'fail reason: ' . $ns_error;
}


