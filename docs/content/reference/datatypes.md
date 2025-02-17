---
title: OSCAL Data Types
heading: Data Types Used in OSCAL
description: A description of the data types used in the OSCAL formats.
weight: 10
suppressintopiclist: true
sidenav:
  title: Data Types
toc:
  enabled: true
aliases:
  - /docs/schemas/datatypes/
  - /documentation/schema/model-concepts/datatypes/
  - /documentation/schema/catalog-layer/datatypes/
  - /documentation/schema/profile-layer/datatypes/
  - /documentation/schema/datatypes/
---

The OSCAL models share a set of data type primitives. These data types are documented in the following sections.

## Simple Data types

### empty

This data type indicates that the model information element contains no value content, but may contain other structured information elements.

In XML, this may represent an element without text content.

In JSON, this may represent an object with labels corresponding to other child information elements, but no label corresponding to a text value.

### boolean

A boolean value mapped in XML, JSON, and YAML as follows:

| Value | XML | JSON | YAML |
|:--- |:--- |:--- |:--- |
| true | `true` or `1` | `true` | `true` |
| false | `false` or `0` | `false` | `false` |

### string

A string of Unicode characters.

### token

A non-colonized name as defined by [XML Schema Part 2: Datatypes Second Edition](https://www.w3.org/TR/xmlschema11-2/#NCName).

### NCName

A non-colonized name as defined by [XML Schema Part 2: Datatypes Second Edition](https://www.w3.org/TR/xmlschema11-2/#NCName).

### integer

An integer value.

OSCAL represents integers [as defined in XSD](https://www.w3.org/TR/xmlschema11-2/#integer).

In JSON Schema, the [`integer` type](https://tools.ietf.org/html/draft-handrews-json-schema-validation-01#section-6.1.1) is used. Additionally, the `multipleOf` keyword is set to `1.0` to ensure an integer value in systems that do not have a native type.

### nonNegativeInteger

An integer value that is equal to or greater than `0`.

In XSD, [nonNegativeInteger](https://www.w3.org/TR/xmlschema11-2/#nonNegativeInteger) is a built in type derived from the `integer` type.

In JSON Schema, this becomes an `integer` value with an additional `minimum` constraint of `0`. Additionally, the `multipleOf` keyword is set to `1.0` to ensure an integer value in systems that do not have a native type.

### positiveInteger

A positive integer value.

In XML Schema, [positiveInteger](https://www.w3.org/TR/xmlschema11-2/#nonNegativeInteger) is a built in type derived from the 'nonNegativeInteger' type.

In JSON Schema, this becomes an `integer` value with an additional `minimum` constraint of `1`. Additionally, the `multipleOf` keyword is set to `1.0` to ensure an integer value in systems that do not have a native type.

### decimal

A real number expressed using decimal numerals.

In XML Schema this is represented as the built in type [decimal](https://www.w3.org/TR/xmlschema11-2/#decimal).

In JSON Schema, this is represented as:

```JSON
{
  "type": "number",
  "pattern": "(\\+|-)?([0-9]+(\\.[0-9]*)?|\\.[0-9]+)"
}
```

## Formatted String Data types

The following are data types based on strings.

### base64Binary

A string representing arbitrary Base64-encoded binary data.

In XML Schema this is represented as the built in type [base64Binary](https://www.w3.org/TR/xmlschema11-2/#base64Binary) until a suitable pattern can be developed.

In JSON Schema, this is represented as:

```JSON
{
  "type": "string",
  "contentEncoding": "base64"
}
```

Once a suitable pattern for XML is developed, this pattern will be ported to JSON for more consistent validation.

### date

In XML, the [date](https://www.w3.org/TR/xmlschema11-2/#date) datatype is used. This is the same as
[date-with-timezone](#date-with-timezone), except the time zone portion is optional.

In JSON, lexical conformance to dates with optional time zones is provided by a regular expression, the same as given above for [date-with-timezone](#date-with-timezone), except as adjusted for the requirement.

### date-with-timezone

A string representing a 24-hour period in a given timezone. A `date-with-timezone` is formatted according to "full-date" as defined [RFC3339](https://tools.ietf.org/html/rfc3339#section-5.6). This type additionally requires that the time-offset (timezone) is always provided.

For example:

```
2019-09-28Z
2019-12-02-08:00
```

In XML Schema this is represented as a restriction on the built-in type [date](https://www.w3.org/TR/xmlschema11-2/#date) as follows:

```XML
<xs:simpleType name="date-with-timezone">
  <xs:annotation>
    <xs:documentation>The xs:date with a required timezone.</xs:documentation>
  </xs:annotation>
  <xs:restriction base="xs:date">
    <xs:pattern value="((2000|2400|2800|(19|2[0-9](0[48]|[2468][048]|[13579][26])))-02-29)|(((19|2[0-9])[0-9]{2})-02-(0[1-9]|1[0-9]|2[0-8]))|(((19|2[0-9])[0-9]{2})-(0[13578]|10|12)-(0[1-9]|[12][0-9]|3[01]))|(((19|2[0-9])[0-9]{2})-(0[469]|11)-(0[1-9]|[12][0-9]|30))(Z|[+-][0-9]{2}:[0-9]{2})(Z|[+-][0-9]{2}:[0-9]{2})"/>
  </xs:restriction>
</xs:simpleType>
```

In JSON Schema, this is represented as:

```JSON
{
  "type": "string",
  "pattern": "((2000|2400|2800|(19|2[0-9](0[48]|[2468][048]|[13579][26])))-02-29)|(((19|2[0-9])[0-9]{2})-02-(0[1-9]|1[0-9]|2[0-8]))|(((19|2[0-9])[0-9]{2})-(0[13578]|10|12)-(0[1-9]|[12][0-9]|3[01]))|(((19|2[0-9])[0-9]{2})-(0[469]|11)-(0[1-9]|[12][0-9]|30))(Z|[+-][0-9]{2}:[0-9]{2})(Z|[+-][0-9]{2}:[0-9]{2})"
}
```

### dateTime

In XML, the [dateTime](https://www.w3.org/TR/xmlschema11-2/#dateTime) datatype is used. This is the same as
[dateTime-with-timezone](#datetime-with-timezone), except the time zone portion is optional.

In JSON, lexical conformance to date-times with optional time zones is provided by a regular expression, the same as given above for [dateTime-with-timezone](#datetime-with-timezone), except as adjusted for the requirement.

### dateTime-with-timezone

A string containing a date and time formatted according to "date-time" as defined [RFC3339](https://tools.ietf.org/html/rfc3339#section-5.6). This type requires that the time-offset (timezone) is always provided. This use of timezone ensure that date/time information that is exchanged across timezones is unambiguous.

For example:

```
2019-09-28T23:20:50.52Z
2019-12-02T16:39:57-08:00
2019-12-31T23:59:60Z
```

In XML Schema this is represented as a restriction on the built in type [dateTime](https://www.w3.org/TR/xmlschema11-2/#dateTime) as follows:

```XML
<xs:simpleType name="dateTime-with-timezone">
  <xs:annotation>
    <xs:documentation>The xs:dateTime with a required timezone.</xs:documentation>
  </xs:annotation>
  <xs:restriction base="xs:dateTime">
    <xs:pattern value="((2000|2400|2800|(19|2[0-9](0[48]|[2468][048]|[13579][26])))-02-29)|(((19|2[0-9])[0-9]{2})-02-(0[1-9]|1[0-9]|2[0-8]))|(((19|2[0-9])[0-9]{2})-(0[13578]|10|12)-(0[1-9]|[12][0-9]|3[01]))|(((19|2[0-9])[0-9]{2})-(0[469]|11)-(0[1-9]|[12][0-9]|30))T(2[0-3]|[01][0-9]):([0-5][0-9]):([0-5][0-9])(\.[0-9]+)?(Z|[+-][0-9]{2}:[0-9]{2})"/>
  </xs:restriction>
</xs:simpleType>
```

In JSON Schema, this is represented as:

```JSON
{
  "type": "string",
  "format": "date-time",
  "pattern": "((2000|2400|2800|(19|2[0-9](0[48]|[2468][048]|[13579][26])))-02-29)|(((19|2[0-9])[0-9]{2})-02-(0[1-9]|1[0-9]|2[0-8]))|(((19|2[0-9])[0-9]{2})-(0[13578]|10|12)-(0[1-9]|[12][0-9]|3[01]))|(((19|2[0-9])[0-9]{2})-(0[469]|11)-(0[1-9]|[12][0-9]|30))T(2[0-3]|[01][0-9]):([0-5][0-9]):([0-5][0-9])(\\.[0-9]+)?(Z|[+-][0-9]{2}:[0-9]{2})"
}
```

### email

An email address string formatted according to [RFC 6531](https://tools.ietf.org/html/rfc6531).

In XML Schema this is represented as the built in type [string](https://www.w3.org/TR/xmlschema11-2/#string) until a suitable pattern can be developed.

In JSON Schema, this is represented as:

```JSON
{
  "type": "string",
  "format": "idn-email",
  "pattern": ".+@.+"
}
```

Once a suitable pattern for XML is developed, this pattern will be ported to JSON for more consistent validation.

### hostname

An internationalized Internet host name string formatted according to [section 2.3.2.3](https://tools.ietf.org/html/rfc5890#section-2.3.2.3) of RFC 5890.

In XML Schema this is represented as the built in type [string](https://www.w3.org/TR/xmlschema11-2/#string) until a suitable pattern can be developed.

In JSON Schema, this is represented as:

```JSON
{
  "type": "string",
  "format": "idn-hostname"
}
```

Once a suitable pattern for XML is developed, this pattern will be ported to JSON for more consistent validation.

### ip-v4-address

An Internet Protocol version 4 address in dotted-quad ABNF syntax as defined in [section 3.2](https://tools.ietf.org/html/rfc2673#section-3.2) of RFC 2673.

In XML Schema this is represented as a restriction on the built in type [string](https://www.w3.org/TR/xmlschema11-2/#string) as follows:

```XML
<xs:simpleType name="ip-v4-address">
  <xs:annotation>
    <xs:documentation>The ip-v4-address type specifies an IPv4 address in dot decimal notation.</xs:documentation>
  </xs:annotation>
  <xs:restriction base="xs:string">
    <xs:whiteSpace value="collapse"/>
    <xs:pattern value="((25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9]).){3}(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9])"/>
  </xs:restriction>
</xs:simpleType>
```

In JSON Schema, this is represented as:

```JSON
{
  "type": "string",
  "format": "ipv4",
  "pattern": "((25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9]).){3}(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9])"
}
```

### ip-v6-address

An Internet Protocol version 6 address in dotted-quad ABNF syntax as defined in [section 2.2](https://tools.ietf.org/html/rfc3513#section-2.2) of RFC 3513.

In XML Schema this is represented as a restriction on the built in type [string](https://www.w3.org/TR/xmlschema11-2/#string) as follows:

```XML
<xs:simpleType name="ip-v6-address">
  <xs:annotation>
    <xs:documentation>The ip-v4-address type specifies an IPv4 address in dot decimal notation.</xs:documentation>
  </xs:annotation>
  <xs:restriction base="xs:string">
    <xs:whiteSpace value="collapse"/>
    <xs:pattern value="(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|[fF][eE]80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::([fF]{4}(:0{1,4}){0,1}:){0,1}((25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9]).){3,3}(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9]).){3,3}(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9]))"/>
  </xs:restriction>
</xs:simpleType>
```

In JSON Schema, this is represented as:

```JSON
{
  "type": "string",
  "format": "ipv6",
  "pattern": "(([0-9a-fA-F]{1,4}:){7,7}[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,7}:|([0-9a-fA-F]{1,4}:){1,6}:[0-9a-fA-F]{1,4}|([0-9a-fA-F]{1,4}:){1,5}(:[0-9a-fA-F]{1,4}){1,2}|([0-9a-fA-F]{1,4}:){1,4}(:[0-9a-fA-F]{1,4}){1,3}|([0-9a-fA-F]{1,4}:){1,3}(:[0-9a-fA-F]{1,4}){1,4}|([0-9a-fA-F]{1,4}:){1,2}(:[0-9a-fA-F]{1,4}){1,5}|[0-9a-fA-F]{1,4}:((:[0-9a-fA-F]{1,4}){1,6})|:((:[0-9a-fA-F]{1,4}){1,7}|:)|[fF][eE]80:(:[0-9a-fA-F]{0,4}){0,4}%[0-9a-zA-Z]{1,}|::([fF]{4}(:0{1,4}){0,1}:){0,1}((25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9]).){3,3}(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9])|([0-9a-fA-F]{1,4}:){1,4}:((25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9]).){3,3}(25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9]))"
}
```

### uri

A universal resource identifier (URI) formatted according to [RFC3986](https://tools.ietf.org/html/rfc3986).

In XML Schema this is represented as the built in type [anyURI](https://www.w3.org/TR/xmlschema11-2/#anyURI) until a suitable pattern can be developed.

In JSON Schema, this is represented as:

```JSON
{
  "type": "string",
  "format": "uri"
}
```

Once a suitable pattern for XML is developed, this pattern will be ported to JSON for more consistent validation.

### uri-reference

A URI Reference (either a URI or a relative-reference) formatted according to [section 4.1](https://tools.ietf.org/html/rfc3986#section-4.1) of RFC3986,

In XML Schema this is represented as the built in type [anyURI](https://www.w3.org/TR/xmlschema11-2/#anyURI) until a suitable pattern can be developed.

In JSON Schema, this is represented as:

```JSON
{
  "type": "string",
  "format": "uri-reference"
}
```

Once a suitable pattern for XML is developed, this pattern will be ported to JSON for more consistent validation.

### uuid

A version 4 Universally Unique Identifier (UUID) as defined by [RFC 4122](https://tools.ietf.org/html/rfc4122).

## Markup Data Types

Structured prose text in OSCAL is designed to map cleanly to equivalent subsets of HTML and Markdown. This allows HTML-like markup to be incorporated in OSCAL XML-based content using an element set maintained in the OSCAL namespace. This HTML-equivalent element set is not intended to be treated directly as HTML, but to be readily and transparently converted to HTML (or other presentational formats) as needed. Similarly, OSCAL uses a subset of Markdown for use in OSCAL JSON- and YAML-based content. A mapping is supported between the HTML-like element set and the Markdown syntax, which supports transparent and lossless bidirectional mapping between both OSCAL markup representations.

The OSCAL HTML-like syntax supports:

- HTML paragraphs (`p`), headers (`h1`-`h6`), tables (`table`), preformatted text (`pre`), code blocks (`code`), and ordered and unordered lists (`ol` and `ul`.)

- Within paragraphs or text content: `a`, `img`, `strong`, `em`, `b`, `i`, `sup`, `sub`.


In remarks below and throughout this documentation, this element set may be referred to as "prose content" or "prose". A future OSCAL could support the definition of this tag set (and Markdown equivalent) as a module, enabling our HTML subset to be switched out for something else. (Its prose model would be different from OSCAL prose as currently defined.)

Note that elements such as `div`, `blockquote`, `section` or `aside`, used in HTML to provide structure, are *not permitted in OSCAL*. Structures in OSCAL should be represented using OSCAL elements (or objects in JSON) such as `part`, which can include prose.

In addition, there are contexts in OSCAL where prose usage may be further constrained. For example, at a higher level (outside the base schema) an OSCAL application could forbid the use of prose headers `h1-h6` in favor of nested OSCAL `part` elements with their own titles.

The OSCAL Markdown syntax is loosely based on CommonMark. When in doubt about Markdown features and syntax, we look to CommonMark for guidance, largely because it is more rigorously tested than many other forms of Markdown.

### markup-line

The following table describes the equivalent constructs in HTML and Markdown used in OSCAL within the `markup-line` data type.

| Markup Type | HTML | Markdown |
|:--- |:--- |:--- |
| Emphasis (preferred) | &lt;em&gt;*text*&lt;/em&gt; | \**text*\*
| Emphasis | &lt;i&gt;*text*&lt;/i&gt; | \**text*\*
| Important Text (preferred) | &lt;strong&gt;*text*&lt;/strong&gt; | \*\**text*\*\*
| Important Text | &lt;b&gt;*text*&lt;/b&gt; | \*\**text*\*\*
| Inline code | &lt;code&gt;*text*&lt;/code&gt; | \`*text*\`
| Quoted Text | &lt;q&gt;*text*&lt;/q&gt; | "*text*"
| Subscript Text | &lt;sub&gt;*text*&lt;/sub&gt; | \~*text*\~
| Superscript Text | &lt;sup&gt;*text*&lt;/sup&gt; | ^*text*^
| Image | &lt;img alt="*alt text*" src="*url*" title="*title text*"/&gt; | !\[*alt text*](*url* "*title text*")
| Link | &lt;a *href*="*url*"&gt;*text*&lt;/a&gt; | \[*text*](*url*)

Note: Markdown does not have an equivalent of the HTML &lt;i&gt; and &lt;b&gt; tags, which indicate italics and bold respectively. These concepts are mapped in OSCAL markup text to &lt;em&gt; and &lt;strong&gt; [common mark](https://spec.commonmark.org/0.29/#emphasis-and-strong-emphasis), which render equivalently in browsers, but do not have exactly the same semantics. While this mapping is imperfect, it represents the common uses of these HTML tags.

#### Parameter Insertion

The OSCAL catalog, profile, and implementation layer models allow for control parameters to be defined and injected into prose text.

Parameter injection is handled in OSCAL as follows using the &lt;insert&gt; tag:

```html
Reviews and updates the risk management strategy <insert param-id="pm-9_prm_1"/> or as required, to address organizational changes.
```

The same string in Markdown is represented as follows:

```markdown
Reviews and updates the risk management strategy {{ pm-9_prm_1 }} or as required, to address organizational changes.
```

#### Specialized Character Mapping

The following characters have special handling in their HTML and/or Markdown forms.

| Character                                      | XML HTML                             | (plain) Markdown | Markdown in JSON | Markdown in YAML |
| ---                                            | ---                                  | ---              | ---              | ---              |
| &amp; (ampersand)                              | &amp;amp;                            | &amp;            | &amp;            | &amp;            |
| &lt; (less-than sign or left angle bracket)    | &amp;lt;                             | &lt;             | &lt;             | &lt;             |
| &gt; (greater-than sign or right angle bracket) | &gt; **or** &amp;gt;                 | &gt;             | &gt;             | &gt;             |
| &#34; (straight double quotation mark)         | &#34; **or** &amp;quot;              | \\&#34;          |  \\\\&#34;       | \\\\&#34;        |
| &#39; (straight apostrophe)                    | &#39; **or** &amp;apos;              | \\&#39;          | \\\\&#39;        | \\\\&#39;        |
| \* (asterisk)                                  | \*                                   | \\\*             | \\\\\*           | \\\\\*           |
| &#96; (grave accent or back tick mark)         | &#96;                                | \\&#96;          | \\\\&#96;        | \\\\&#96;        |
| ~ (tilde)                                      | ~                                    | \\~              | \\\\~            | \\\\~            |
| ^ (caret)                                      | ^                                    | \\^              | \\\\^            | \\\\^            |

While the characters ``*`~^`` are valid for use unescaped in JSON strings and YAML double quoted strings, these characters have special meaning in Markdown markup. As a result, when these characters appear as literals in a Markdown representation, they must be escaped to avoid them being parsed as Markdown to indicate formatting. The escaped representation indicates these characters are to be represented as characters, not markup, when the Markdown is mapped to HTML.

Because the character "\\" (back slash or reverse solidus) must be escaped in JSON, note that those characters that require a back slash to escape them in Markdown, such as "\*" (appearing as "\\\*"), must be *double escaped* (as "\\\\\*") to represent the escaped character in JSON or YAML. In conversion, the JSON or YAML processor reduces these to the simple escaped form, again permitting the Markdown processor to recognize them as character contents, not markup.

Since these characters are not markup delimiters in XML, they are safe to use there without special handling. The XML open markup delimiters "&lt;" and "&amp;", when appearing in XML contents, must as always be escaped as named entities or numeric character references, if they are to be read as literal characters not markup.

### markup-multiline

All constructs supported by the [markup-line](#markup-line) data type are also supported by the `markup-multiline` data type, when appearing within a header (`h1`-`h6`), paragraph (`p`), list item (`li`) or table cell (`th` or `td`).

The following additional constructs are also supported. Note that the syntax for these elements must appear on their own lines (i.e., with additional line feeds as delimiters), as is usual in Markdown.

| Markup Type | HTML | Markdown |
|:--- |:--- |:--- |
| Heading: Level 1 | &lt;h1&gt;*text*&lt;/h1&gt; | # *text*
| Heading: Level 2 | &lt;h2&gt;*text*&lt;/h2&gt; | ## *text*
| Heading: Level 3 | &lt;h3&gt;*text*&lt;/h3&gt; | ### *text*
| Heading: Level 4 | &lt;h4&gt;*text*&lt;/h4&gt; | #### *text*
| Heading: Level 5 | &lt;h5&gt;*text*&lt;/h5&gt; | ##### *text*
| Heading: Level 6 | &lt;h6&gt;*text*&lt;/h6&gt; | ###### *text*
| Preformatted Text | &lt;pre&gt;*text*&lt;/pre&gt; | \`\`\`*text*\`\`\`
| Ordered List, with a single item | &lt;ol&gt;&lt;li&gt;*text*&lt;/li&gt;&lt;/ol&gt; | 1. *text*
| Unordered List with single item | &lt;ul&gt;&lt;li&gt;*text*&lt;/li&gt;&lt;/ul&gt; | - *text*

#### Paragraphs

Additionally, the use of `p` tags in HTML is mapped to Markdown as two double, escaped newlines within a JSON or YAML string (i.e., "\\\\n\\\\n"). This allows Markdown text to be split into paragraphs when this data type is used.

#### Tables

Tables are also supported by `markup-multiline` which are mapped from Markdown to HTML as follows:

- The first row in a Markdown table is considered a header row, with each cell mapped as a &lt;th&gt;.
- The alignment formatting (second) row of the Markdown table is not converted to HTML. Formatting is currently ignored.
- Each remaining row is mapped as a cell using the &lt;td&gt; tag.
- HTML `colspan` and `rowspan` are not supported by Markdown, and so are excluded from OSCAL.

OSCAL attempts to support simple tables mainly due to the prevalence of tables in legacy data sets. However, producers of OSCAL data should note that when they have tabular information, these are frequently semantic structures or matrices that can be described directly in OSCAL as named parts and properties or as parts, sub-parts and paragraphs. This ensures that their nominal or represented semantics are accessible for processing when this information would be lost in plain table cells. Table markup should be used only as a fallback option when stronger semantic labeling is not possible.

Tables are mapped from HTML to Markdown as follows:

* Only a single header row &lt;tr&gt;&lt;th&gt; is supported. This row is mapped to the Markdown table header, with header cells preceded, delimited, and terminated by `|`.
* The second row is given as a sequence of `---`, as many as the table has columns, delimited by single `|`. In Markdown, a simple syntax here can be used to indicate the alignment of cells; OSCAL HTML does not support this feature.
* Each subsequent row is mapped to the Markdown table rows, with cells preceded, delimited, and terminated by `|`.

For example:

The following HTML table:

```html
<table>
  <tr><th>Col A</th><th>Col B</th></tr>
  <tr><td>Have some of</td><td>Try all of</td></tr>
</table>
```

Is mapped to the Markdown table:

```markdown
| Col A | Col B |
| --- | --- |
| Have some of | Try all of |
```


#### Line feeds in Markdown

Additionally, line feed (LF) characters must be escaped as "\\n" when appearing in string contents in JSON and (depending on placement) in YAML. In Markdown, the line feed is used to delimit paragraphs and other block elements, represented using markup (tagging) in the XML version. When transcribed into JSON, these LF characters must also appear as "\\n".
