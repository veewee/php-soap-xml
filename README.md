# SOAP XML

This package provides some tools on top of [veewee/xml](https://github.com/veewee/xml)'s DOM component in order to make it easier to deal with SOAP XML structures.

## Builder

### SoapHeaders

Makes it possible to build the content of a `soap:Header` element.

```php
use Soap\Xml\Builder\SoapHeaders;
use Soap\Xml\Manipulator\PrependSoapHeaders;use VeeWee\Xml\Dom\Document;
use function VeeWee\Xml\Dom\Builder\children;
use function VeeWee\Xml\Dom\Builder\element;
use function VeeWee\Xml\Dom\Builder\value;

$doc = Document::fromXmlString($xml);
$builder = new SoapHeaders(
    children(
        element('user', value('josbos')),
        element('password', value('topsecret'))
    )
);

$header = $doc->build($builder)[0];

// You can prepend the soap:Header as first element of the soap:envelope
// Like this
$doc->manipulate(new PrependSoapHeaders($header));
```

## Locator

### BodyNamespaceLocator

Locates the namespace of the first element inside the soap:Body.

```php
use Soap\Xml\Locator\BodyNamespaceLocator;
use VeeWee\Xml\Dom\Document;

$doc = Document::fromXmlString($xml);
$bodyNamespace = $doc->locate(new BodyNamespaceLocator());
```

### SoapBodyLocator

Locates the `soap:Body` element inside a `soap:Envelope`

```php
use Soap\Xml\Locator\SoapBodyLocator;
use VeeWee\Xml\Dom\Document;

$doc = Document::fromXmlString($xml);
$bodyElement = $doc->locate(new SoapBodyLocator());
```


### SoapEnvelopeLocator

Locates the `soap:Envelope` inside XML.

```php
use Soap\Xml\Locator\SoapEnvelopeLocator;
use VeeWee\Xml\Dom\Document;

$doc = Document::fromXmlString($xml);
$bodyElement = $doc->locate(new SoapEnvelopeLocator());
```

### SoapHeaderLocator 

Locates the `soap:Header` element inside a `soap:Envelope`

```php
use Soap\Xml\Locator\SoapHeaderLocator;
use VeeWee\Xml\Dom\Document;

$doc = Document::fromXmlString($xml);
$bodyElement = $doc->locate(new SoapHeaderLocator());
```

## Manipulator

### PrependSoapHeaders

See SoapHeaders builder:

```php
$doc = Document::fromXmlString($xml);
$doc->manipulate(new PrependSoapHeaders($soapHeader));
```

## XPath

### EnvelopePreset

This preset allows you to use following xpath prefixes:

- `application`: The namespace of the SOAP implementation.
- `soap`: The soap prefix allows you to fetch common elements like Body, header, Envelope, ...

```php
use Soap\Xml\Xpath\EnvelopePreset;
use VeeWee\Xml\Dom\Document;

$doc = Document::fromXmlString($xml);
$xpath = $doc->xpath(new EnvelopePreset($doc));

$xpath->querySingle('/soap:Envelope');
$xpath->querySingle('//soap:Body');
$xpath->querySingle('//soap:Header');
$xpath->querySingle('//soap:Body//application:Foo');
```


### WsdlPreset

This preset allows you to use following xpath prefixes:

- `wsdl`: The wsdl prefix allows you to fetch common elements like definitions, types, services, operations, ...

```php
use Soap\Xml\Xpath\WsdlPreset;
use VeeWee\Xml\Dom\Document;

$doc = Document::fromXmlString($xml);
$xpath = $doc->xpath(new WsdlPreset($doc));

$xpath->querySingle('/wsdl:definitions');
$xpath->querySingle('//wsdl:types');
$xpath->querySingle('//wsdl:services');
```
