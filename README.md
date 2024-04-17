# XML parser and writer

An XML parser and writer with namespace support. Packaged as a JavaScript ES6 module. Somewhat brittle.

## Usage examples

_Tip: Run the examples below by typing this in your terminal (requires Deno):_

```shell
deno run \
  --allow-net --allow-run --allow-env --allow-read \
  https://deno.land/x/mdrb@2.0.0/mod.ts \
  --dax=false --mode=isolated \
  https://raw.githubusercontent.com/doga/xml/master/README.md
```

<details data-mdrb>
<summary>Example: Parse and re-generate an XML string.</summary>

<pre>
description = '''
Running this example is safe, it will not read or write anything to your filesystem.
'''
</pre>
</details>

```javascript
import { Parser, Writer } from 'https://esm.sh/gh/doga/xml@1.0.0/mod.mjs';

const 
xml = 
`<!-- 
  A Qworum script that calls a login endpoint.
  The Web application uses the Qworum browser extension
  to run the script.
-->
<sequence xmlns='https://qworum.net/ns/v1/instruction/'>
  <try>
    <call href='/login' />
    <catch faults='["* cancelled"]'>
      <goto href='/loginCancelled' />
    </catch>
    <catch faults='["* failed"]'>
      <goto href='/loginFailed' />
    </catch>
  </try>
  <goto href='/account' />
</sequence>`,

doc = new Parser(xml).document, // XmlDocument

// serialise the root element
xml2 = Writer.elementToString(doc.root);

console.info(xml2);
```

Sample output for the code above:

```text
<sequence xmlns="https://qworum.net/ns/v1/instruction/">
  <try>
    <call href="/login"></call>
    <catch faults="[&quot;* cancelled&quot;]">
      <goto href="/loginCancelled"></goto>
    </catch>
    <catch faults="[&quot;* failed&quot;]">
      <goto href="/loginFailed"></goto>
    </catch>
  </try>
  <goto href="/account"></goto>
</sequence>
```

<details data-mdrb>
<summary>Example: Build an XML document programmatically.</summary>

<pre>
description = '''
Running this example is safe, it will not read or write anything to your filesystem.
'''
</pre>
</details>

```javascript
import { Parser, Writer, XmlDocument, XmlElement, XmlComment } from 'https://esm.sh/gh/doga/xml@1.0.0/mod.mjs';

const 
doc = new XmlDocument([
  new XmlComment(
    `A Qworum script that calls a login endpoint.
    The Web application uses the Qworum browser extension
    to run the script.`
  ),
  new XmlElement(
    'sequence', {xmlns: 'https://qworum.net/ns/v1/instruction/'},
    [
      new XmlElement(
        'try', {},
        [
          new XmlElement('call', {href: '/login'}),
          new XmlElement(
            'catch', {faults: '["* cancelled"]'},
            [new XmlElement('goto', {href: '/loginCancelled'})]
          ),
          new XmlElement(
            'catch', {faults: '["* failed"]'},
            [new XmlElement('goto', {href: '/loginFailed'})]
          ),
        ]
      ),
      new XmlElement('goto', {href: '/account'}),
    ]
  )
]),

// serialise the root element
xml = Writer.elementToString(doc.root);

console.info(xml);
```

Sample output for the code above:

```text
<sequence xmlns="https://qworum.net/ns/v1/instruction/"><try><call href="/login"></call><catch faults="[&quot;* cancelled&quot;]"><goto href="/loginCancelled"></goto></catch><catch faults="[&quot;* failed&quot;]"><goto href="/loginFailed"></goto></catch></try><goto href="/account"></goto></sequence>
```

∎
