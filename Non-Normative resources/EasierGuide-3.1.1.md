# Easier Guide to the AAVLD/USAHA and NASAHO Electronic CVI Data Standards

Michael K Martin, DVM, MPH, DACVPM

# Preface

This guide is meant to fill in gaps in the common understanding of how
electronic certificates of veterinary inspection (eCVI) interoperate in
the digital world. This interaction involves policy makers who develop
the rules for interstate movement, state and federal regulators who
enforce those rules, software designers who build eCVI applications, and
the technical programmers who write the actual computer code that makes
data exchange possible. Members of each of these groups have ample
knowledge of their own domain but usually little or no information about
others. In this guide, everyone will find material in their own field
oversimplified while the coverage of the others is detailed. The parts
of this guide that are not in the reader’s domain are intended to
provide a high-level understanding. Programmers will learn some of the
policy behind the XML details. Policy makers will understand a bit about
where XML schema can help and where it can’t. My goal has been to ease
the reader into the details by starting at a very high level and adding
detail as the guide goes on. It is a good idea to read, at least
superficially, the parts that do not apply to your own work so that the
later discussion will make sense. If, while reading the parts in which
you are an expert, you find errors, please let me know at
<Michael.martin.dvm.mph@gmail.com>. I will confirm and make corrections in
future editions.

To emphasize that this is not a normative document, I have used first
person throughout. As the sole author, I have used first person singular
for my interpretation, opinion, and advice. I have tried to be explicit
when recalling decisions, opinions, etc. of others including the
standards committees.

When I taught programming for the University of Maryland (Asian
Division), my department chairman accused me of not caring if my code
worked as long as it was beautiful. I don’t think he meant that entirely
as an insult. Computer code is a living thing that needs maintenance
over the years. Making it accessible to human programmers goes a long
way to ensuring quality. The eCVI standard reads like it was written by
a committee over a decade . . . because it was! In this guide I point
out some of that ugliness—much of which is on me as one of the
editors—to try to make them make sense to a new reader of the code.

The first edition of this guide covered version 3.0 of the standard. I
have occasionally included reference to earlier versions for
clarification but have not tried to provide comprehensive explanations
of earlier versions. In this update, I have added explanations of
changes in version 3.1 and will continue to do so for future minor
versions. Any major future versions may require completely new editions.

The topic of this guide centers on computer code so use of non-plain-English is unavoidable. Some formatting conventions may help distinguish between computer language and English text. When I use variable names or example code from the standards, they are formatted as `inline code` using backticks. For longer code examples and actual XML code samples throughout the guide, I use fenced code blocks with syntax highlighting:

```xml
<example>
  <element attribute="value">content</element>
</example>
```

In the text, literal string values from the standard are included in quotation marks. Grammar rules say that the period at the end of a sentence that ends with a quote goes inside the quotation mark. When what is inside the quote is example XML content, I break this rule to avoid confusion that the period is part of the data. The actual values in the sample code blocks follow the standard when they are constrained by lists or patterns, but I had some fun with the values in fields that allow any string. One should never copy sample code directly from any source but especially here.

# Table of Contents

[Preface](#preface)

[Table of Contents](#table-of-contents)

[Introduction](#introduction)

[What the AAVLD/USAHA Standard _Is_ and What it Is _Not_](#what-the-aavldusaha-standard-is-and-what-it-is-not)

[The National Assembly of State Animal Health Officials (NASAHO) Role](#the-national-assembly-of-state-animal-health-officials-nasaho-role)

[What This Guide _Is_ and What it Is _Not_](#what-this-guide-is-and-what-it-is-not)

[XML and XML Schema Review](#xml-and-xml-schema-review)

[XML Documents and Schemas](#xml-documents-and-schemas)

[Regular Expressions](#regular-expressions)

[Entities](#entities)

[Whitespace](#whitespace)

[Namespaces](#namespaces)

[Working With XML](#working-with-xml)

[XML and XML Schema as the eCVI Standard](#xml-and-xml-schema-as-the-ecvi-standard)

[Reuse Planning and Future Proofing](#reuse-planning-and-future-proofing)

[Common Elements](#common-elements)

[Type Definitions](#type-definitions)

[Root Document Elements](#root-document-elements)

[Other Derivative Documents](#other-derivative-documents)

[Images and Other Attachments](#images-and-other-attachments)

[Optionality of Information](#optionality-of-information)

[Additional Optionality Considerations](#additional-optionality-considerations)

[Tiptoe Through the Tags](#tiptoe-through-the-tags)

[The XML Header](#the-xml-header)

[XML Version](#xml-version)

[Character Encoding](#character-encoding)

[Root Document Element Opening Tag](#root-document-element-opening-tag)

[High-level Overview of the eCVI Element](#high-level-overview-of-the-ecvi-element)

[eCVI Element Attributes](#ecvi-element-attributes)

[XMLSchemaVersion](#xmlschemaversion)

[CVINumber](#cvinumber)

[CVINumberIssuedBy](#cvinumberissuedby)

[IssueDate](#issuedate)

[ExpirationDate](#expirationdate)

[ShipmentDate](#shipmentdate)

[EntryPermitNumber](#entrypermitnumber)

[ReplacesCVINumber](#replacescvinumber)

[Voided](#voided)

[eCVI Child Elements](#ecvi-child-elements)

[Veterinarian](#veterinarian)

[MovementPurposes](#movementpurposes)

[Origin](#origin)

[Destination](#destination)

[Consignor](#consignor)

[Consignee](#consignee)

[Carrier](#carrier)

[TransportMode and TransportModeOtherDescription](#transportmode-and-transportmodeotherdescription)

[Accessions](#accessions)

[Animal, GroupLot, and Product](#animal-grouplot-and-product)

[Statements](#statements)

[Attachment](#attachment)

[MiscAttribute](#miscattribute)

[Binary](#binary)

[Details of Elements and Complex Types](#details-of-elements-and-complex-types)

[Person](#person)

[Veterinarian](#veterinarian-1)

[MovementPurposes](#movementpurposes-1)

[USAddress](#usaddress)

[InternationalAddress](#internationaladdress)

[Origin, Destination, PremType](#origin-destination-premtype)

[StateZoneOrAreaStatus](#statezoneorareastatus)

[HerdOrFlockStatus](#herdorflockstatus)

[Consignor, Consignee, ContactType](#consignor-consignee-contacttype)

[Animal](#animal)

[GroupLot](#grouplot)

[Product](#product)

[Closer Look at Some Complex Child Elements](#closer-look-at-some-complex-child-elements)

[SpeciesCode and SpeciesOther](#speciescode-and-speciesother)

[AnimalTags](#animaltags)

[Accession](#accession)

[Test](#test)

[Vaccination](#vaccination)

[CountryOfBirth](#countryofbirth)

[Attachment](#attachment-1)

[MiscAttribute](#miscattribute-1)

[Binary](#binary-1)

[Regular Expression Patterns Used in the Schema Explained](#regular-expression-patterns-used-in-the-schema-explained)

[Enumerated Value Lists Used in the Schema](#enumerated-value-lists-used-in-the-schema)

[Transmission and Other NASAHO Requirements](#transmission-and-other-nasaho-requirements)

[Additional Data Element Requirements](#additional-data-element-requirements)

[Electronic Signature 64](#electronic-signature)

[Rendering of the Data as PDF and XML](#rendering-of-the-data-as-pdf-and-xml)

[Delivery](#delivery)

[License and Accreditation](#license-and-accreditation)

[Information Technology Support](#information-technology-support)

[Flexibility](#flexibility)

[Supported Variations](#supported-variations)

[Movement](#movement)

[NPIP Movements](#npip-movements)

[Sighting](#sighting)

[Suggestions on Software Development Practices](#suggestions-on-software-development-practices)

[XML Development Process](#xml-development-process)

[Correct Data Starts with the User Interface](#correct-data-starts-with-the-user-interface)

[Conditionality](#conditionality)

[“Almost Required” Data Items](#almost-required-data-items)

[Abuse of the “Other” Value](#abuse-of-the-other-value)

[Regulatory Compliance](#regulatory-compliance)

[Finally, On the Way Out . . . (Validation)](#finally-on-the-way-out-.-.-.-validation)

[Appendices](#appendices)

[A: Premises Identification Number Check Digit Validation Algorithm](#a-premises-identification-number-check-digit-validation-algorithm)

[ISO 7064, Mod 37, 36](#iso-7064-mod-37-36)

[Formula for Calculating the Check Digit for a 7 Character Identifier](#formula-for-calculating-the-check-digit-for-a-7-character-identifier)

[Over Simplified Java Language Implementation](#over-simplified-java-language-implementation)

[B: Electronic Signature Considerations](#b-electronic-signature-considerations)

[Electronic Signature Requirements](#electronic-signature-requirements)

[Electronic Signature Technologies](#electronic-signature-technologies)

[Regulatory Environment](#regulatory-environment)

[Conclusions](#conclusions)

# Introduction

Historically, health certificates—now called certificates of veterinary
inspection (CVI)—were, as the name implies, used to document that
animals or groups of animals had been inspected and found free of signs
of disease. These were used in animal sales and often required to
prevent spread of diseases during movement of animals between states. As
paper documents, CVIs were used for the transaction or shipment and then
largely forgotten. Then in the mid two thousand aughts, partly because
of America’s first case of mad cow disease, it became clear that the
United States needed a better way to trace animal movements for disease
response.

Certificates of veterinary inspection became official movement documents
by accident. The only existing records that referenced interstate
movement were CVIs. Multiple attempts to create a national animal
movement database proved politically unacceptable. In 2011 the U.S.
Department of Agriculture proposed a rule requiring CVIs for all
interstate movement of most livestock. While CVIs document the health
qualification of an animal or group to move rather than the actual
movement, they were the best thing available and politically acceptable.
But CVIs were still copies of paper in file cabinets. Using them for
animal disease tracing was very labor-intensive and prohibitively slow.
In some cases, officials spent days searching for a single animal
movement record in paper CVIs. They were also inconvenient for
veterinarians to complete by hand or typewriter.

A technology industry began to emerge to create electronic versions of
the state CVI forms. These electronic CVIs (eCVI) were faster and more
convenient for veterinarians, but still needed to be delivered to state
animal health officials (“state vets”) and each state had to approve
them as equivalent to state-issued paper forms. In parallel with the
development of eCVIs, state veterinarian’s offices were converting from
paper files to database applications. These animal health databases made
finding information much faster once it had been entered into the
computer. That left a gap between eCVIs created electronically and the
state animal health database applications. The result was much double
data entry as the eCVI information was retyped into database
applications. A common data format was needed to allow direct
transmission of the _information_ in the eCVIs into the animal health
databases.

In 2012 the Committee on Animal Health Surveillance and Information
Systems, a joint committee of the American Association of Veterinary
Laboratory Diagnosticians (AAVLD) and the United States Animal Health
Association (USAHA), created a subcommittee—now called a working
group—to develop a standard for transmission of eCVI data. This
working group follows the American National Standards Institute (ANSI)
requirements for standards development. [^1] It uses an open,
industry-driven, consensus standard development process. Development of
the standard has involved active participation, and leadership, from
both eCVI and animal health database developers as well as state and
federal officials. It has received input and guidance from academic
veterinary informatics specialists familiar with medical informatics
standards development processes. The first version of the standard was
released in 2013, followed by version 2 in 2017, and with minor updates
yearly. Most recently, in January 2024, version 3 added support for
related data transactions for animal sightings and movements other than
CVIs. Due for release in January 2025, version 3.1 adds a few minor
features and corrects some very trivial errors.

## What the AAVLD/USAHA Standard _Is_ and What it Is _Not_

The initial driving force behind creation of the eCVI data standard was
the need to exchange animal disease traceability information for
interstate animal movement within the United States. International
movements, both import and export, involve a very different set of
requirements, different data, and are managed by different government
authorities. Because of this, the standard is explicitly for movements
within the U.S. and its territories. Where appropriate, certain entities
can be outside the U.S., but the animal movements must have origin and
destination in the U.S.

The workgroup initially focused on the data requirements for regulated
movement of livestock. It quickly became apparent that the same data
structures would support companion animal CVIs and that many, if not
most, eCVI applications support both from the same platform. More
recently, support for certain non-live animal products such as semen and
hatching eggs that require a CVI for shipment was added. The standard
does not directly support what the USDA rule calls “alternative movement
documents,” however these may reuse significant elements of the eCVI
standard. More information on reuse is included later.

The standard defines the data format that eCVIs export so they can be
seamlessly imported into various state animal health databases. There
must be an unambiguous way to determine if any given document (file or
equivalent) complies with the defined format. The workgroup explored
multiple options before settling on XML and XML schema language for the
data files and the standard respectively. These two underlying standards
are briefly reviewed in the next chapter.

The standard does not specify storage or transmission protocols. It does
not specify any electronic signature or other security mechanism. It
does not give any guidance as to when and where CVIs may be required.
And it does not even require distribution of the data to any party or
parties. Most importantly, the standard says nothing about how eCVI
applications should function. It is, of course, hoped that these will
provide help to veterinarians as they complete the required
documentation. The more these applications improve, the easier it will
be for veterinarians to generate valid, high-quality data using the
applications. But those application details are left to the companies
and developers providing the eCVI software. The standard _only_
specifies the format for exported data from individual eCVI documents.

Much of the work of the workgroup takes place on GitHub and is
documented in the Issues part of the eCVI project. The contents of the
standard have all been developed by consensus of the workgroup, ensuring
balanced representation from all interest categories. The actual text of
the XML schema was written and edited over the years by workgroup
volunteers functioning as editors. Each proposed addition or change
submitted by the workgroup was drafted by the editor and placed
(committed) in a git branch labeled PendingChanges. Prior to each
release, the workgroup reviewed each pending change and approved or
rejected via consensus. Prior to release, the approved changes are added
to the main branch and a release created.

The standard is, quite simply, the XML schema for the **eCVI** XML data
element. To know if a given data file complies with the standard, one
must validate it against the current standard XML schema.

## The National Assembly of State Animal Health Officials (NASAHO) Role

Besides the definition of a standard data exchange format, broad
acceptance of eCVIs requires approval by all or at least a large
majority of state veterinarians. To help meet that need the National
Assembly of State Animal Health Officials (NASAHO or National Assembly)
established a committee to define requirements for eCVI applications,
including **but much more than** generating data that validates against
the standard XML Schema. Many of the items explicitly excluded from the
AAVLD/USAHA standard are essential for a functioning eCVI ecosystem. The
NASAHO committee has provided guidance on these issues. Developers of
eCVI applications can apply to the National Assembly to have their
application evaluated. Member states retain authority to accept or
reject any given eCVI application for use in their state but have agreed
that they all accept the committee’s approval as sufficient for state
acceptance.

## What This Guide _Is_ and What it Is _Not_

**This guide is not the standard. It must not be used to demonstrate
compliance. The only way to demonstrate compliance is to use XML
validation tools to show that the data file in question validates
against the current schema.**

The only problem with saying that the XML schema _is_ the standard, is
that many interested parties are not intimately familiar with the
details of XML schema language or have the time and interest to
carefully read and understand the raw code. Even some developers who are
familiar with XML and XML schema may not immediately grasp the intended
usage of each part of the standard.

This conflict has been apparent since the standard was first published
in 2013. Most state veterinarians and USDA officials do not “speak XML”
fluently. Various efforts have been made to translate the XML schema
code into somewhat human-readable language. Those were somewhat useful
but failed to capture intent of the various parts of the standard. And,
of course, they did nothing to provide guidance on the aspects that are
outside the standard such as best practices for data processing,
transmission, etc. some of which have been addressed by the National
Assembly.

In this guide I will attempt to pull together the content of the
standard with the workgroup’s intent for usage and future-proofing. I
will explain some of the more complex or confusing XML structures and
why those were used. Finally, I will address the requirements developed
by the National Assembly for more general approval of eCVI applications
as equivalent to all state CVI forms and acceptable nationally.

The additional movement and sighting documentation that is included in
the standard schema are only briefly discussed here as they relate to
the main eCVI standard root element.

# XML and XML Schema Review

This chapter is not intended to be complete introduction to either XML
or XML schema. There are many good books on both. Instead, I briefly
introduce these underlying standards as used in the eCVI standard. If
you are interested only in the content of the standard rather than need
to implement it directly, then feel free to skim this chapter. It should
help you follow the discussion later but shouldn’t be necessary to get
the gist of the content.

## XML Documents and Schemas

XML is part of a long line of what are called “markup languages.” The X
stands for eXtensible (nerds get cute with acronyms sometimes.) Markup
languages include both text and instructions about what to do with the
text. These instruction tags can do various things. In hypertext markup
language (HTML) they define how the text should be displayed in a web
browser, import images, and link to other files. In extensible markup
language (XML) the tags provide context for the _meaning_ of the
enclosed text. For example, a date may be inside tags that define it as
the patient’s birthdate. XML documents can stand on their own but are
much more powerful when the structure of tags and content is defined in
a document type definition (DTD). The most widely used form of DTD is a
standard XML document called an XML schema. Both XML and XML schema are
defined in standards from the World Wide Web Consortium (W3C).

Elements are the building blocks of XML. An XML document is made of a
root element containing additional elements. Elements are enclosed in
tags, such as `<element>content</element>`, where the opening tag
`<element>` and the closing tag `</element>` define the boundaries of
the element. The content between the tags carries the actual data. The
content may be additional elements or text.

Attributes provide additional information about elements. They are
included within the opening tag and are defined as name-value pairs. For
example, `<person age="30">John Doe</person>` represents an element
person with an attribute age having a value of “30”.

Because the tags in XML define the elements’ meaning, XML makes an ideal
language in which to define the contents of other XML documents. An XML
schema is an XML document, the contents of which define the order and
content of a class (type) of XML documents that it defines. Thus, an XML
schema is a Document Type Definition written in XML. If you aren’t
already dizzy, the content and structure of XML schema documents is
defined in an XML schema written by the W3C.

A schema defines the sequence, nesting, choices, and data types of
elements and attributes in the document. Simple data types can be
created starting with data types defined in the XML standard itself by
adding constraints such as lists or patterns of valid values. More
complex data types can be created by listing elements and attributes or
by extension of existing complex types adding elements or attributes.

This snippet of XML schema defines an element named `E`. That element must
contain two child elements `A` and `B` as well as two attributes `c` and `d`.
The names of the elements in the schema itself—starting with xs:-- are
what tell the computer (parser) what `E`, `A`, `B`, `c`, and `d` _are_.

```xml
<xs:element name="E">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="A" type="xs:string"/>
      <xs:element name="B" type="xs:string"/>
    </xs:sequence>
    <xs:attribute name="c" type="xs:string"/>
    <xs:attribute name="d" type="xs:string"/>
  </xs:complexType>
</xs:element>

```

The next box has three examples of XML document content. They are all
element `E` but only the first one is a valid example of what is defined
in the schema above. The second one is missing the element `B`. The third
one has an extra attribute `f`.

```xml
<E c="1" d="2">
  <A>3</A>
  <B>4</B>
</E>
<E c="1" d="2">
  <A>3</A>
</E>
<E c="1" d="2" f="5">
  <A>3</A>
  <B>4</B>
</E>

```

Some XML elements may be empty. That is, they may have no text or child
elements, but only attributes. Such elements have only one tag that ends
with a slash character such as `<emptyElement attr="true"/>`. The only
data the element named emptyElement gives us is that the attribute
called attr is true.

## Regular Expressions

Most computer users are familiar with the concept of “wild cards.” In
many searches, an asterisk (\*) character can be used to mean “anything
can go here.” Many power-user tools and programming languages include a
much more powerful way of defining pattern matching. Regular Expressions
(RegEx) are strings of characters and special characters that divide
candidate strings into those that match and those that don’t. For
example, `\d{2}[A-Z]{2}\d{4}` matches NEUS8 tag values—two digits, two
capital letters, and four digits—but no other strings.

To define and test new regular expressions takes some skill and
practice. Even reading more complex regular expressions is not always
easy. The eCVI standard includes `xs:documentation` elements in the
definitions of all the types that use regular expressions in their
definitions. I explain the regular expressions used in the standard at
the end of this guide.

## Entities

The text inside elements and attributes is technically “parsed character
data” but that is more detail than we need here. It just means that the
software reading the XML can look for special characters, etc.

Possibly the most confusing part of XML encoding is what to do about
characters that can’t be included in parsed character text because they
would be, well, “parsed.” Things like “\<” would be read as the start of
a tag. Entities are multiple character groups that stand in for these
special characters.

XML entities start with the ampersand (&) character and end with a
semicolon. In between is coding that tells what character is being
replaced. For example, our “<” would be `&lt;` (for less than). The
ampersand itself would be `&amp;` so we wouldn’t confuse it with the
start of another entity.

Your favorite XML book will have much more to say about entities and
many more characters that can be encoded with them.

## Whitespace

Whitespace is composed of any of several characters that don’t normally
show in a printed document. That is, the paper would still be white.
This includes spaces, tabs, and new line characters. When these appear
between the opening and closing XML element tags, they are part of the
data. When they appear between elements, however, they are ignored. You
often will see XML documents displayed or printed to make them more
human-readable. You will hear this called “pretty printing.” This
whitespace formatting is usually omitted in the transmitted documents to
save a very small amount of space. It makes no difference to the
software processing the XML either way.

## Namespaces

One last advanced XML topic I need to just touch on here is
“namespaces.” XML has a way to allow names in an XML document to be
globally unique without having to make them long and unreadable. Have
you tried making up a unique gmail address lately? The XML language
allows names to be qualified with a namespace. The namespace is based on
an internet address of some kind, that makes it unique. But those are
usually hidden away and replaced by simple abbreviation prefixes or set
once as a default for the whole document. A bit more on this later but
for now, when you see xs: before a name, that means it is a name defined
in the W3C schema namespace.

## Working With XML

Because XML is plain text with text markup, it can be edited in any text
editor. Many general-purpose editors provide some special support for
XML or have add-ons that do so. There are also specific XML editors that
provide powerful features for creating and analyzing XML documents and
XML schemas. Pretty printing is a very common feature of most XML aware
text editors. The color in the XML schema and document examples in this
guide was added by my XML editor as “syntax highlighting.” It is not
part of the documents themselves. One very useful, but less common,
feature of a good XML editor is the ability to quickly find the
definitions for elements included by type or by reference. The most
important feature is the ability to evaluate an XML document to ensure
that it has consistent structure (is “well formed”) and complies with an
XML schema (is “valid” with reference to the schema). XML validation can
also be performed in software using XML support libraries. Because XML
schema 1.0 has been so widely supported for so long, such libraries are
available for virtually any programming language and environment. The
only way to know if an eCVI is in compliance with the standard is to
check it against the standard eCVI XML schema in an editor or program
that does validation.

# XML and XML Schema as the eCVI Standard

The information included in any XML document exists in either of two
forms, elements or attributes. Within the XML design world there are
different opinions about the correct mix of element text versus
attributes to convey various types of information. The eCVI standard
uses a mixture. Attributes are mainly simple strings, numbers, or dates
that are part of the item described by the element such as animal age.
Included elements include complex data or things that may exist in
multiples such as animal identifiers. There is no perfect set of design
choices, and everyone can find something where they would have made a
different choice. But consensus is that the design choices _work_ so
should be left as is.

In this guide we show samples of an imaginary eCVI XML document in boxes
like this:

```xml
<Animal Age="0d" Breed="Breed1" Sex="Female" InspectionDate="2006-05-04">
  <SpeciesCode Code="AQU" Text="Text1"/>
  <AnimalTags>
    <AIN Number="840000000000000"/>
  </AnimalTags>
  <Test AccessionRef="ID000" TestCode="TestCode1">
    <Result ResultName="RESULT">
      <ResultInteger> 0 </ResultInteger>
    </Result>
  </Test>
  <Vaccination Type="Type49" Date="2006-05-04"/>
  <Vaccination Type="Type51" Date="2006-05-04"/>
</Animal>
```

This snippet includes one Animal element from a fictional eCVI. The
Animal element has attributes for `Age`, `Breed`, `Sex`, and `InspectionDate`.
It has included elements for a list of `AnimalTags`. We only have one tag
in this example, but it allows for more. It also has one Test and two
Vaccination elements. The details don’t matter now, we’ll tiptoe through
all these tags in a later detailed chapter.

XML schemas can define the structure of an element in line, defining
each sub-element as it appears in the parent element. This can become a
long list of deeply nested definitions and can be both unreadable and
hard to maintain. It can also define each of the sub-elements on its own
and include them in the parent element by reference. Elements with
simple data types are most often included in-line. Complex element types
are generally implemented as element definitions and then included by
reference. They are defined as types when usage may vary between
elements.

Extracts of the actual schema will also show in boxes. A giveaway that
you are looking at schema is all the `xs:` prefixes. Our schema creates an
abbreviation `xs:` for the namespace `“http://www.w3.org/2001/XMLSchema”` so
you can tell that the names that follow are part of the schema language
rather than something the workgroup made up. Here W3C made up the name
element but the workgroup made up the name PremId.

```xml
<xs:complexType name = "PremType">
  <xs:annotation>
    <xs:documentation> PremType is used for origin and destination, and must be actual physical … </xs:documentation>
  </xs:annotation>
  <xs:sequence>
    <xs:element name = "PremId" type = "PremIdType " />
    <xs:element name = "PremName" type = "xs:string " />
    <xs:element name = "Address" type = "USAddress " />
    <xs:element ref = "StateZoneOrAreaStatus" minOccurs = "0" maxOccurs = "unbounded" />
    <xs:element ref = "HerdOrFlockStatus" minOccurs = "0" maxOccurs = "unbounded" />
    <xs:element ref = "Person" minOccurs = "0" maxOccurs = "unbounded" />
  </xs:sequence>
</xs:complexType>
```

Here the definition of a `PremType` includes `PremId`, `PremName`, and `Address`
defined here based on two locally defined types (`PremIdType` and
`USAddress`) and one XML type `xs:string`. It includes three more elements
by reference to elements defined by themselves (`StateZoneOrAreaStatus`,
`HerdOrFlockStatus`, and `Person`). Again, don’t worry about the details
now. Just know that you will see different styles for defining elements
and there is _usually_ a logical reason for the choice.

Notice the `xs:documentation` element in the example above. The standard
schema includes many of these elements. They have no effect on document
validation but provide guidance on the intended usage of the elements
they are contained in. I have removed these documentation elements from
the examples for brevity and because they duplicate some of what I cover
in the text.

Also, notice one unusual—and probably wrong—feature in the standard.
Spaces are not allowed in variable names. But for readability, multiword
names are often useful. Both elements and attributes have names in what
is called “camel case” with each word in the name capitalized. Modern
XML style guides generally have attributes starting with lower case and
elements starting with upper case. Here the standard starts names with
upper case in both. This has absolutely no functional effect but makes
it somewhat harder to tell them apart by name only. Note, however, that
XML _is_ case sensitive, so eCVI data must use the case defined in the
schema.

All these details work together to define what is and what is not a
valid eCVI data document. **_To repeat: The only way to know if an eCVI
is in compliance with the standard is to check that it is valid with
respect to the standard eCVI XML schema._**

# Reuse Planning and Future Proofing

The eCVI standard schema has been in development for over a decade and
the current format for almost as long. In some places it shows the
quirks that come from making enhancements while minimizing impact on
existing implementations. For the most part, however, the schema was
designed to facilitate this evolution as smoothly as possible. In some
cases, the perfect solution evaded consensus and the workgroup elected
to leave things as flexible as possible for future work. This chapter
will cover some of the high-level design choices to add some clarity to
why things are laid out in the schema as they are.

### Common Elements

The bulk of the schema is made up of free-standing element definitions.
Most of these represent “things” that will be familiar from the
information in CVIs, electronic or not. These include things like
origin, destination, animal, animal tags, as well as more basic things
like email and address. These elements have names that should be
obvious.

These are defined as stand-alone elements because they do, or may,
appear in different locations in the standard eCVI schema or even in
derivative definitions such as the new sighting and movement document
types.

The overall structure of the standard document, and related documents,
can be understood by reference to the top-level elements contained in
them.

### Type Definitions

In most cases the decision whether to represent a “thing” as a complex
type-definition or as a stand-alone element is arbitrary. The working
group made most “things” stand-alone elements. A few are defined as
complex types. Sometimes, such as concepts like “premises” occur in
distinctly different elements like “origin” and “destination.” In some
cases, there are twists such as addresses existing in two variations,
one for U.S. address and another for international address.

Many of the detailed requirements of the standard are included in what
are called “simple types.” These are called “simple” because they
consist of only a single value. But they are distinct types because they
include both the basic data type such as string or number but also much
more specific restrictions such as a list (enumeration) of allowed
values, or a regular expression (pattern) that they must match.

Simple types can be used to constrain element text or attribute values.
The goal has been to constrain each item to only valid values. In a few
cases, definitions are left as the XML string type because either no
unambiguous format was available, or no consensus existed on a list of
valid codes. It is hoped that future releases may further constrain
these items.

Some elements that have values constrained to an allowed list of values
also include an “other” choice and a sub-element to provide a value.
These are included as sparingly as possible but are unavoidable in some
cases. For example, it is important that animal species use standard
codes whenever possible, but it is impossible to list every exotic
species that might be moved with a CVI. While not enforceable via XML
schema language and therefore not technically part of the standard, it
is wrong to use “other” for any value that is included in the enumerated
list.

A final—hopefully rarely used—extensibility feature is the ability to
add unconstrained name-value pairs of information at the end of the
document. As with use of “other” this feature is intended to allow use
of the standard format in extreme edge cases where a specific type of
information becomes essential, and until the standard can be updated to
include the same in more structured format. It may also prove useful to
support value-added features that are out of scope for the standard but
useful additions to the standard eCVI.

### Root Document Elements

Until version 3.0, the standard schema included only one root document
element `eCVI`. Version three added two additional document types,
`Movement` for documentation of generic animal movement not involving
veterinary certification, and `Sighting` for documentation of an animal’s
or group of animals’ location on a given date.

These additional root elements are not technically part of the eCVI data
standard but are included in the same schema file for practical reasons.
XML schema language provides the ability to “include” or “import” an
external schema within another by reference. If not handled very
carefully, however, this capability can lead to errors. When referenced
by network address, this can even cause serious security risks. For this
reason, and to keep things as simple and practical as possible, the
workgroup elected to maintain these extensions in the main schema.

### Other Derivative Documents

While outside the scope of the eCVI data standard or the AAVLD/USAHA
data standards workgroup, it is hoped, and already seen, that the schema
will be useful in guiding development of related animal location and
movement information. Those who use the schema in this way should be
aware that the workgroup will make changes to the definitions as needed
to stay consistent with federal and state regulations, and to constrain
to the greatest extent possible the content of compliant documents to
legal values. Every effort will be made to keep such changes as
backward-compatible as possible but need for some maintenance on
derivative works can be expected.

### Images and Other Attachments

XML is a strictly text—UTF-8 in this case—medium. CVIs often include
pictures, such as horse photographs and brand images. They may also have
attachments such as PDFs of laboratory reports. These items are binary
and often quite large. Binary files can be encoded in plain text using a
format called “base64” that represents each three bytes of binary
content as four ordinary ASCII characters. This base64 string is often
accompanied by a code for its file type such as “image/jpeg” or
“application/pdf.”

The eCVI schema puts all such binary content in one element type,
`Binary`. This element includes the base64 text, an optional file type,
and most importantly an xs:ID attribute to allow it to be referenced in
any of the more specific elements with binary content. It is hoped that
this allows applications to implement the encoding and decoding once for
reuse in multiple contexts. It also keeps the long, un-human-readable
base64 at the end. That has minimal impact on computer processing but
can be very helpful for human reading and debugging during development.

# Optionality of Information

Deciding whether any specific data item must be included in a standard
document or message is one of the most challenging parts of implementing
any interoperability standard. This has been true since the earliest
days of electronic data exchange. Another word for this is “cardinality”
which also includes the number of times a value may repeat. For the eCVI
data standard, cardinality is established on multiple levels.

Being required in the XML schema means that the item must exist in
_every_ valid document. Optional items may still be very important but
have specific cases in which they are omitted. This shows up for
elements and attributes. Elements have a minimum and maximum number of
allowed occurrences. If the minimum is one or more, the element must
exist in every eCVI. Attributes do not repeat within an element, so
their usage is listed as optional or required. Again, if required, it
means that every eCVI must have a value assigned to that attribute.
Because the eCVI XML schema _is_ the standard, this is as far as the
standard can enforce requiredness. (More advanced schema languages such
as `XML schema 1.1`, `RelaxNG`, or `Schematron`, can enforce conditional
requiredness by referencing other parts of the XML document so, for
example, something might be required for cattle but not for horses. But,
as discussed later, use of these would have significantly reduced the
number of programming languages and environments where the schema could
be used.)

Sometimes an element may be optional but, if included, has required
child elements or attributes. The meaning in these cases is that if the
required content does not exist, the entire element must be omitted.
This is done in cases where the outer optional element doesn’t make
sense without its required content. This is a common source of error in
much XML programming.

### Additional Optionality Considerations

Both USDA and the National Assembly have requirements for data items
that must appear in CVIs depending on various conditions. Unless they
must appear in every CVI, the standard schema will not be able to
enforce these. But NASAHO may make compliance with these conditional
items part of its evaluation. The NASAHO committee also requires that
any item that is included on the printed CVI and that has a
corresponding element/attribute in the standard schema must be populated
in the data file. More on the NASAHO evaluation process later.

The `xs:documentation` elements in the schema often provide guidance on
conditional requiredness beyond what is enforceable in schema language.

# Tiptoe Through the Tags

## The XML Header

The XML header on both the schema and documents must read exactly  
`<?xml version="1.0" encoding="UTF-8"?>`. This never changes.

But don’t ignore the header. Both of those values are important. Both
XML version and character encoding are a common sources of processing
errors.

### XML Version

The eCVI standard uses XML version 1.0 and XML schema version 1.0. These
were published in 1998 and 2001 respectively and are very widely
supported. Versions 1.1 of each are newer and include some potentially
useful features but are not as universally supported. Because the
strength of the eCVI standard comes from the widest possible
application, the difference in level of support is more important than
feature richness. The good news is that you need not shop for the latest
tools or books on either standard. The eCVI uses only common and
long-standing features of XML schema. The same logic explains why the
standard is not implemented with newer—and more fashionable—tools like
Java Script Object Notation (JSON).

### Character Encoding

The second piece of key information in the header is the character
encoding used. At the risk of sounding pedantic, let’s briefly review
what that means.

Computers do not store letters and numbers but only ones and zeros,
called “bits.” Eight bits make a byte with the one’s bit on the right
and the 128’s bit on the left. At least in English, and for several
decades now the first 127 characters have been pretty well established
as encoded in ASCII. The first (highest) bit being zero in ASCII. To
encode special characters such as accented letters, additional bits are
needed, and there have been numerous different ways to add them. The
most common now is Unicode. Unicode comes in different flavors. Almost
all XML (and _all_ JSON) is encoded in UTF-8. This is a variable width
code. When the first bit is a zero, it is the same as ASCII and uses one
byte per character. Since most of XML fits into normal ASCII, this makes
it space-saving. Extended characters are encoded by using more than one
byte signaled by the first bit being 1 (and complicated coding after
that). Good. So, what is the worry?

Because space is not a big issue anymore in most computers, many
operating systems and programming languages routinely use UTF-16 or some
other multi-byte encoding. A common error in programming with XML is to
ignore the character encoding. This may result in either of two
problems. The sending system may encode the characters in its native
UTF-16. This will look completely normal in most editors but as a stream
of bytes will not match what a properly functioning receiving system
expects. Or the receiving system, ignoring the encoding instruction and
seeing what look like ASCII characters, gets tripped up by the first
extended character such as Ñ. One might write a program using default
settings and test with many documents that _do_ contain only seven-bit
ASCII and not discover this until processing something with accented
letters, etc.

## Root Document Element Opening Tag

We can’t totally ignore some important XML housekeeping that goes on in
the opening tags of both our eCVIs and the eCVI schema, so bear with a
little more XML esoterica in the next section.

Any XML element can declare itself and all its contents to belong to a
default namespace. This is most often done in a root document element,
`eCVI` in our case. Even though the current schema is version 3.1, the
namespace has not changed since version 2.0. All the names in version 3
that existed in version 2 still mean the same things. Thus, the
namespace is still listed as `http://www.usaha.org/xmlns/ecvi2`. The first
line after the header will usually read  
`<eCVI xmlns=http://www.usaha.org/xmlns/ecvi2`  
Notice that there is no “>” at the end of this line. The opening
element keeps going.

Note that the definitions of namespaces often _look_ like URLs. They are
actually URIs. What is the difference? Universal Resource Identifiers
don’t necessarily point to anything on the internet. They just provide a
unique name. Because the domain naming system does a good job of
maintaining uniqueness, URLs make good URIs for namespaces. But they
don’t mean, “Look here for the namespace” or anything like that.

Optionally, but helpful during manual development, information about the
location of the schema can be included in the root element tag. For
security reasons, we discourage including a network address here (or in
any include, import, etc.) but a local copy of the schema can be
identified. Many good XML editors use that information to validate as
you edit and to offer type-ahead. So my hand edited files include

```xml
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.usaha.org/xmlns/ecvi2
file:ecvi2.xsd"
```

as the next two lines. The first one defines the prefix `xsi:` as a
namespace and the second uses the `schemaLocation` attribute from that
namespace to say we want to use the file ecvi2.xsd from the current
directory to define the namespace we put ourselves in above.

The opening element of our schema root element is similar but adds the
definition of the `xs:` prefix as XML schema names, declares the schema
namespace _and_ target namespace to be our
“http://www.usaha.org/xmlns/ecvi2” address, and the version to be “3.1
”. The bit about `elementFormDefault="qualified"` just means use the
schema namespace for element and attribute names. Now don’t worry about
most of the declarations here. The most important is the
`XMLSchemaVersion="3.1"` to be sure you are validating against the right
version of the schema. If you are still actually validating against the
previous version 3.0 this, of course, needs to be
`XMLSchemaVersion="3.0"`.

NOTE: When you see an ellipsis . . . in these boxes, it means something
more goes here but we will come back to that later or have already seen.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<eCVI xmlns = "http://www.usaha.org/xmlns/ecvi2"
  xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation = "http://www.usaha.org/xmlns/ecvi2 file:ecvi2.xsd" XMLSchemaVersion = "3. 1 " . . . >
```

Okay, that is about as nerdy as we are going to get. Now back to things
that actually apply to CVIs. But, of course, the next section will start
with another XML weirdness.

## High-level Overview of the eCVI Element

When we look at an element in an XML document, the attributes—those
little single value name-value pairs—come in the opening tag. That is,
they come at the beginning. On the other hand, when we look at the
schema, the sequence of child elements comes first followed by the list
of attributes. No one knows what the folks at W3C were taking when they
made that rule, but it must have been good. In this guide, I will
usually follow the order in the document and discuss attributes first.
Just know that in an element with both child elements and attributes you
look for the attributes at the _end_ of the `xs:complexType` definition in
the schema.

## eCVI Element Attributes

The eCVI element includes some key housekeeping facts in attributes.

### XMLSchemaVersion

We have seen this attribute earlier. It should always be the value found
in the schema’s attribute `version` for the schema you are designing and
validating against. You _are_ validating against the schema in test,
right? And ideally in production. Validating parsers really are fast
enough to make this practical.

### CVINumber

Nothing much to say here. This is the unique identifier displayed on the
CVI as printed and used by anyone looking for a specific CVI. This
attribute is required. Note that the data type is not just `xs:string` but
a defined simple type `nonNullString`. That means that the value cannot be
empty or just whitespace. It really must have a meaningful value.

### CVINumberIssuedBy

Because CVI numbers can be duplicated by different states when they
print their forms or by different eCVI applications, we need to know the
pool of numbers the CVI number was drawn from. For state paper CVIs,
this would be the state postal code. For eCVI applications, it must be a
string that uniquely identifies the application in a way that ensures
that the `CVINumberIssuedBy` combined with the `CVINumber` will _never_ be
repeated. This attribute is still optional as of version 3.1 but is
highly encouraged because it is very useful in search, duplicate
detection, etc. Some receiving applications go so far as to add it to
their internal copy of the data when receiving from a known source.

### IssueDate

This is the date that the CVI was signed. For revisions and voids this
is the date that the revision was issued or the CVI was voided. The only
thing to note here is that dates in the eCVI use the XML standard date
format: Four-digit year, dash, two-digit month, dash, two-digit day of
month. (YYYY-MM-DD). This is required and because the format is
specified, may not be blank.

### ExpirationDate

Expiration date of the CVI. This is a complex calculation based on
various rules using the inspection dates of the animals and the issue
date. It is generally thirty days from the earliest animal inspection
date. This is required and may not be blank.

### ShipmentDate

This is the _anticipated_ date that the animal(s) will leave the origin
premises. This attribute is optional. But review the definition of
optional before ignoring it. If it appears on the CVI, the NASAHO
committee will require it in the XML data. I won’t repeat this warning
for every optional element/attribute but applies throughout.

### EntryPermitNumber

If an entry permit identifier has been issued, it goes here. Because
these identifiers can take many forms this is just defined as a string.

### ReplacesCVINumber

Now this gets complicated. In the days of paper CVIs, once the form was
issued and passed on, there was no good way to note changes. This was
often a change in destination address but sometimes addition or removal
of animals. Electronic CVI applications have more control but must still
ensure integrity of the data. This attribute and the next are used to
choreograph the data dance. The formal process follows that which should
be used in the paper world. First, the original CVI must be marked as
void, and then the replacement issued. This was very time consuming in
the paper world but can be quite efficient in software.

The main difference between a replacement CVI and a new CVI is that the
replacement relies upon the same veterinary inspection of the animals.

The `ReplacesCVINumber` attribute contains _exactly_ the value from the
`CVINumber` attribute of the eCVI being replaced. This attribute is
optional, but conditional on the action being taken.

### Voided

In the case of a replaced CVI, this attribute indicates that the eCVI
identified by `ReplacesCVINumber` has been voided.

In the case when a CVI is voided without replacement, `ReplacesCVINumber`
is omitted and the attribute marks the current eCVI as void.

This attribute is optional, but conditional on the action being taken.

```xml
<xs:element name = "eCVI"> . . .  <xs:attribute name = "XMLSchemaVersion" type = "nonNullString" use = "required" / >
    <xs:attribute name = "CviNumber" type = "nonNullString" use = "required" />
    <xs:attribute name = "CviNumberIssuedBy" type = "xs:string" use = "optional" />
    <xs:attribute name = "IssueDate" type = "xs:date" use = "required" / >
      <xs:attribute name = "ExpirationDate" type = "xs:date" use = "required" />
      <xs:attribute name = "ShipmentDate" type = "xs:date" use = "optional" />
      <xs:attribute name = "EntryPermitNumber" type = "xs:string" use = "optional" />
      <xs:attribute name = "ReplacesCviNumber" type = "nonNullString" use = "optional" />
      <xs:attribute name = "Voided" type = "xs:boolean" default = "false" use = "optional" / > . . . </xs:element>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<eCVI xmlns = "http://www.usaha.org/xmlns/ecvi2"
  xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation = "http://www.usaha.org/xmlns/ecvi2 file:ecvi2.xsd" XMLSchemaVersion = "3. 1 " CviNumber = " SC001234 " CviNumberIssuedBy = " ZoomCVI " IssueDate = "202 5 -04-28" ExpirationDate = "202 5 -0 5 -28" ShipmentDate = "202 5 -0 4 - 30 " EntryPermitNumber = "zybLi9D6VqRZi" ReplacesCviNumber = " SC001233 " Voided = " true ">
```

## eCVI Child Elements

Next, we will look at the XML Elements that nest inside the root eCVI
element. Most of these are complex, structured data. I will get to those
details later as we drill down in a chapter on each complex element.

### Veterinarian

The `Veterinarian` element doesn’t need a lot of explanation. The
veterinarian is the licensed and accredited individual veterinarian that
signed the original CVI. The method of adding a signature, electronic or
otherwise, is outside the scope of the eCVI XML standard. There is one
and only one veterinarian on a CVI. If something looks like a CVI and
acts like a CVI but doesn’t have a veterinarian, then it may be a
generic movement document.

### MovementPurposes

This element is fairly straightforward in XML terms but quite
complicated in the real-world. Why is the animal or group being moved?
It would be possible to drill down to any level of detail depending on
why one was collecting the information. For purposes of this standard,
the movement purposes are needed because many states have variations on
their import requirements based on why the animal or group is entering
their state. The combination of individual purposes listed may someday
facilitate computer decision support and other tools to help avoid
violations of these requirements.

### Origin

The origin is the physical location from which an animal or group of
animals on the CVI are expected to move based on the inspection and
documentation represented by this CVI. Historically, the consignor was
the first part of the “sender” section of the paper CVI with something
like “physical address, if different” tacked on at the end. With the
decision to make CVIs the country’s main source of movement information,
the physical locations became more important than the seller and buyer
data. So, origin became the mandatory part.

The `Origin` element is a `PremType` element representing the animal
premises as defined by the USDA animal disease traceability program
where the animal(s) were located prior to movement. It must exist once
and only once.

### Destination

Similar to origin, destination is now the mandatory element rather than
consignee or buyer. This gets more complicated in the real-world because
details of the final destination are sometimes finalized at the last
minute or even after a load has left the origin.

The `Destination` is another `PremType` element that must exist once and
only once.

```xml
<xs:element name = "eCVI"> . . .  <xs:complexType>
    <xs:sequence>
      <xs:element ref = "Veterinarian" minOccurs = "1" maxOccurs = "1" />
      <xs:element ref = "MovementPurposes" minOccurs = "1" maxOccurs = "1" />
      <xs:element ref = "Origin" minOccurs = "1" maxOccurs = "1" />
      <xs:element ref = "Destination" minOccurs = "1" maxOccurs = "1" />

```

```xml
<eCVI . . . >
  <Veterinarian . . . > . . . </Veterinarian>
  <MovementPurposes> . . . </MovementPurposes>
  <Origin> . . . </Origin>
  <Destination > . . . </Destination>
```

### Consignor

The consignor is the business or person responsible for initiating the
movement. This may be the seller or the owner of the origin farm, etc.
Historically, this was the most important information on the “from” end
of the transaction. The move to using CVIs as the primary movement
document changed this, and the origin premises is now the more important
element, so `Consignor` is now optional. And can be used to send the name
and address of the consignor especially if different from that in the
origin premise element. If the same, it may be included but need not be.
The `Consignor` is defined as a `ContactType` element that can occur zero or
one time. We will cover the definition of `ContactType` later.

### Consignee

The same logic and structure apply to `Consignee` and `Consignor`. This is
the person or business responsible for receiving the shipment.

### Carrier

The `Carrier` is the person or business responsible for the physical
movement of the animal(s), group(s) or product(s). It is a `ContactType`
element that may occur once or may be omitted. Note, however, the
National Assembly rule that if carrier occurs on the printed CVI, this
element must be populated.

### TransportMode and TransportModeOtherDescription

The mode of transport takes up two elements. The first `TransportMode` is
a string from a short list of methods of transport. This may be “air”,
“boat”, “car”, “rail”, “truck”, or “land”. If the mode of transport is
anything else, the value here may be “other”. While XML schema language
1.0 does not provide for enforcement, if `TransportMode` is “other”, then
`TransportModeOtherDescription` must be included and populated with more
than whitespace (`nonNullString`). (Note: this could be refactored to be
similar to `SpeciesCode` and `SpeciesOther` but for now the mode of
transport is not critical enough to warrant the extra development work.)

```xml
<xs:element name = "eCVI"> . . .  <xs:complexType>
    <xs:sequence> . . .      <xs:element ref = "Consignor" minOccurs = "0" maxOccurs = "1" />
      <xs:element ref = "Consignee" minOccurs = "0" maxOccurs = "1" />
      <xs:element ref = "Carrier" minOccurs = "0" maxOccurs = "1" />
      <xs:element ref = "TransportMode" minOccurs = "0" maxOccurs = "1" />
      <xs:element ref = "TransportModeOtherDescription" minOccurs = "0" maxOccurs = "1" />

```

```xml
<eCVI . . . > . . .
<Consignor> . . . </Consignor>
<Consignee> . . . </Consignee>
<Carrier> . . . </Carrier>
<TransportMode> other<TransportMode>
<TransportModeOtherDescription> Flying saucer<TransportModeOtherDescription>
```

### Accessions

We don’t usually think of a list of lab accessions as being a top-level
part of a CVI. They are included here to reduce duplication in the
records for animal tests. In herd shipments one laboratory accession—or
one field testing event—often includes all, or most of, the animals in a
herd or flock. By putting accessions here, they can be included by
reference in each of the animal tests. We will cover that mechanism in
detail later. The `Accessions` element is a single list of zero or many
`Accession` elements. If empty, the whole `Accessions` element may be
omitted.

### Animal, GroupLot, and Product

The heart of the eCVI is a list of animals, groups of animals, and
animal products that have been inspected and listed for shipment. The
distinction between `Animal` and `GroupLot` is a little more complicated
than it first seems. This is based on the USDA rules for animal disease
traceability found in 9CFR86, etc. Some animals require individual
official, unique identification that must be included on the CVI. That
requirement is what defines `Animal` in the eCVI schema.

There are many exceptions to the individual identification requirement.
`GroupLot` is for animals that fit any of those exceptions. Most, but not
all, of those are animals that move as a group. Some of those have
requirements for an official Group Identification Number (GIN). Others
may not require official identification at all or may require it but not
require recording on the CVI. All those examples, even a single animal
not requiring official ID on the CVI, go in `GroupLot`. If an animal that
does not _require_ unique official animal ID on the CVI, nevertheless
has it, it would be included as an `Animal` element. Note that 9CFR86
requires that CVIs for animals that do not require official
identification on the CVI state the exemption that applies. This
information must be provided in the description of the `GroupLot`. Thus,
we will see, when we look at the structure in detail, that Description
is one of the few required elements.

If a group includes more than one animal, the information in the
`GroupLot` element must apply to all the animals in the group. If not, it
must be divided into groups that do. Most of the information in `GroupLot`
is optional but that does not allow for simply omitting the variables
where differences occur because of the National Assembly requirement for
all information on the printed CVI to be transmitted.

`Product` is a third type of entity that may move on a CVI. These are
animal-derived products that have animal health significance. These
include things like embryos, semen, and hatching eggs. A full list is
included later when we detail the structures of these three elements.

`Animal`, `GroupLot`, and `Product` can repeat in any combination and order.
There must, obviously, be at least one of any of these. Otherwise, there
would be no point to the CVI.

```xml
<xs:element name = "eCVI"> . . .  <xs:element ref = "Accessions" minOccurs = "0" maxOccurs = "1" />
  <xs:choice minOccurs = "1" maxOccurs = "unbounded">
    <xs:element ref = "Animal" />
    <xs:element ref = "GroupLot" />
    <xs:element ref = "Product" />
  </xs:choice>
```

```xml
<eCVI . . . > . . .
  <Accessions>
    <Accession . . . > . . . </Accession>
    <Accession . . . > . . . </Accession>
  </Accessions>
  <Animal . . . > . . . </Animal>
  <GroupLot . . . > . . . </GroupLot>
  <Animal . . . > . . . </Animal>
  <Product . . . > . . . </Product>
```

### Statements

Structurally, the simplest element in the entire schema, `Statements` is
perhaps the hardest to get right in the larger definition of “right.”
This is a single, optional, simple `xs:string` field. It is designed to
contain specific text that veterinarians need to include to meet
specific movement requirements. These are often imposed on movements
from areas with temporary disease concerns. They can usually be found at
https://InterstateLivestock.com. That site is provided by the USAHA and
the National Institute for Animal Agriculture (NIAA) with content
maintained and updated by state veterinary officials.

While again not enforceable by XML schema, this element _must not_ be
used to send any information for which there are structured data
elements. It is not for herd status, lists of IDs, vaccinations, etc.

### Attachment

CVIs often have additional documents as attachments. Only some of those
apply to eCVIs. The rules in 9CFR86 allow for some of the information,
such as lists of animals with their identification, to be included as
attachments. In an electronic world this doesn’t make sense. Those data
must be sent in the intended, structured form.

This also does not include photographs of horses. Those have a
structured location within the `Animal` element.

Other attachments such as Coggins and other test records, may be
included as binary attachments using the `Attachment` element that is
optional and may repeat. We will cover the detailed construction of
binary attachments later.

Allowing binary attachments was a controversial topic when first
introduced to the workgroup. There was significant concern that
attachments might be used instead of providing complete structured data.
On the other hand, images of original source documentation can sometimes
provide clues needed to successfully conclude tracing, etc. Thus,
attachments must be used only as a supplement, not a replacement for
full structured content.

### MiscAttribute

If it is required on a CVI, it does not belong in miscellaneous
attributes. This element is included for two basic reasons. One is
future-proofing. If some new requirement comes along without sufficient
warning for a structured data implementation to be included in a release
of the eCVI standard, those data could go here temporarily. It is hoped
that will never happen. The workgroup would make every effort to release
a proper implementation as soon as possible.

That leaves the main reason for this element. Some eCVI implementations
may want to include value-added information in the eCVI data they
provide. This element provides a way for them to do so by arrangement
with receiving systems while complying with the schema. Other receiving
systems should be able to safely ignore this information.

`MiscAttribute` is a simple, empty element with two `xs:string` attributes:
`Name` and `Value`. So, anything that can be represented as text can be sent
here.

### Binary

The `Binary` element would never be used on its own. It is included in the
eCVI by reference from any of several elements that have binary rather
than ordinary text content. The `Binary` element itself holds just the
base64 encoded payload and a little information about what it is. It or
they appear(s) at the very end of the eCVI because it is big and
completely not human-readable.

```xml
<xs:element name = "eCVI"> . . .  <xs:element ref = "Statements" minOccurs = "0" maxOccurs = "1" />
  <xs:element ref = "Attachment" minOccurs = "0" maxOccurs = "unbounded" />
  <xs:element ref = "MiscAttribute" minOccurs = "0" maxOccurs = "unbounded" />
  <xs:element ref = "Binary" minOccurs = "0" maxOccurs = "unbounded" />

```

```xml
<eCVI . . . > . . .
  <Statements> Animals have not been exposed to kryptonite </Statements>
  <Attachment . . . />
  <Attachment . . . />
  <MiscAttribute Name = "ShipmentWeight " Value = " 105tons " />
  <MiscAttribute Name = "MaxSpeed " Value = " mac4 " />
  <Binary . . . >
   . . .
   </Binary>
 </eCVI>
```

## Details of Elements and Complex Types

Here we are going to drill down into each of the elements that make up
the eCVI down to the level of individual data items.

### Person

Lawyers like to talk about “legal persons” and “natural persons.” A
“natural person” is what we think of as “people.” On the other hand, a
“legal person” is any entity that may have standing in a court. This
generally pulls in actual people but also businesses, corporations, even
animals, and even a river in New Zealand. And what would an “illegal
person” be, Jesse James? “Person” in the eCVI standard means a business
or natural person. It includes most of the data you would expect in a
contact card.

The most interesting detail is the name of the person. This was a
compromise between the splitters and lumpers. The element starts with
either a `Name` element that is a simple string or a NameParts element.
This allows implementations that distinguish the name parts to transmit
that detail but does not require those that collect it as a single “full
name” string to parse it. It is easier for a receiving application to
construct a full name from parts than a sending system to parse the
other way. If the sending system has the name parsed into parts, they
should be sent in `NameParts` rather than concatenated into Name.

Name parts consists of `BusinessName`, `FirstName`, `MiddleName`, `LastName`,
and `OtherName` each of which is a string and may be omitted. This is not
quite the fully-structured name that informaticists use to support
internationalization but reduces much ambiguity.

The rest of `Person` consists of contact methods: `Phone`,
`InternationalPhone`, and `Email`. The first two require a little
explanation. In the interest of data-quality, the standard constrains
simple data types as tightly as possible. Because this is a US standard,
most phone numbers will follow the consistent ten-digit pattern we are
all used to. Internationally the picture gets much more complex. Rather
than allow every international pattern all the time, the standard
requires international phone numbers to go in their own element defined
by a very complicated RegEx pattern that we will discuss later. There
can be any number of `Phone` and/or `InternationalPhone` elements.

Besides the number, both `Phone` and `InternationalPhone` include an
optional string attribute `Comment`, and an optional `Type` attribute that
allowed values, “Unknown”, “Landline”, “Cellphone”, and “Fax”. Why would
one include this attribute only to list the value as “Unknown?” Type was
previously required but as people have made less and less distinction
between cell phones and landlines, and as faxes have faded, “Unknown”
became the default answer in most applications. So, this attribute was
changed to optional. In the continuing spirit of simplifying the
transition, the previous implementation continues to be supported. A new
eCVI implementation would probably omit `Type` if none is collected in the
user interface.

The `Email` element is a one string element defined by `EmailType`. This
simple type restricts the value to follow a RegEx that means, more or
less, anystring@anystring.anystring. Detailed explanation will come
later. Any number of `Email` elements may occur.

```xml
<xs:element name = "Person">
  <xs:complexType>
    <xs:sequence>
      <xs:choice>
        <xs:element ref = "NameParts" minOccurs = "1" maxOccurs = "1" />
        <xs:element name = "Name" type = "xs:string" minOccurs = "1" maxOccurs = "1" />
      </xs:choice>
      <xs:element ref = "Phone" minOccurs = "0" maxOccurs = "unbounded" />
      <xs:element ref = "InternationalPhone" minOccurs = "0" maxOccurs = "unbounded" />
      <xs:element ref = "Email" minOccurs = "0" maxOccurs = "unbounded" />
    </xs:sequence>
  </xs:complexType>
</xs:element>
<xs:element name = "NameParts">
  <xs:complexType>
    <xs:sequence>
      <xs:element name = "BusinessName" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
      <xs:element name = "FirstName" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
      <xs:element name = "MiddleName" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
      <xs:element name = "LastName" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
      <xs:element name = "OtherName" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

```xml
<Person>
  <NameParts>
    <BusinessName> Shipper Inc. </BusinessName>
    <FirstName> Sally </FirstName>
    <MiddleName> Q </MiddleName>
    <LastName> Shipper </LastName>
    <OtherName> von Shiple </OtherName>
  </NameParts>
  <Phone Number="2290040270" Type="Cellphone"/>
  <InternationalPhone Number="981234567" Comment="Intergalactic hyperphone"/>
  <Email Address="Sally@shipper.com"/>
  <Email Address="Shipping@shipper.com"/>
</Person>
```

### Veterinarian

A veterinarian is a natural person, and we will see, later in this
section, how that affects the standard schema.

The `Veterinarian` element has attributes for `LicenseState`, `LicenseNumber`,
and `NationalAccreditationNumber`. These are all currently optional. This
is one place where it would pay to be future-proofing. Besides the
NASAHO requirement for all items printed to be in the data, these items,
especially the accreditation number, are becoming more and more
important to the regulatory officials on the receiving end. The
regulations in 9CFR161.7 “Activities performed by non-accredited
veterinarians” provide rare exceptions, such as military veterinarians,
to the requirement for national accreditation. Other than those
exceptions, we may expect one or all of these to become required in a
future version of the standard.

```xml
<xs:element name = "Veterinarian">
  <xs:complexType> . . .    <xs:attribute name = "LicenseState" type = "xs:string" use = "optional" />
    <xs:attribute name = "LicenseNumber" type = "xs:string" use = "optional" />
    <xs:attribute name = "NationalAccreditationNumber" type = "xs:string" use = "optional" />
  </xs:complexType>
</xs:element>
```

```xml
<Veterinarian LicenseState = " SC " LicenseNumber = " 2403 " NationalAccreditationNumber = " 001234 " > . . .
```

The `Veterinarian element` has two child elements, `Person` and `Address`. And
now it gets interesting.

There is a `Person` element defined at the top level of the schema but
that is _not_ used here. `Veterinarian` redefines the `Person` element to be
exactly like the top-level `Person` except that `FirstName` and `LastName` in
the `NameParts` element become required. They must each occur once and
only once. This is to emphasize that the veterinarian must be a real,
natural person. Why would the standard do this rather than define two
types for legal and natural persons? The reason is the workgroup’s
commitment to minimize the impact of changes on existing
implementations. By the time someone noticed a few instances of eCVIs
with veterinary practices listed as the veterinarian, there were already
many eCVIs out there, using `Person` correctly in both cases. The strange
looking redefinition of `Person` in just the `Veterinarian` element allowed
all correct instances to remain valid while invalidating only those with
just a `BusinessName`. All that said, it is still possible, but wrong, to
send just a business by using the unstructured `Name` element instead of
`NameParts`. But please don’t do that.

And why aren’t natural person and legal person defined as top-level
complex types and used in the various locations that way? Who knows?
This standard has been developed over more than a decade and with three
different primary editors. Many schema design choices are almost
completely a matter of style—"mine is beautify, yours is just ugly”—and
in another environment each iteration would update the style to match
the new work. But here such changes would add effort for the
implementers of the standard, so the consensus is to leave them. And the
change would make absolutely no difference in validation of eCVI
documents.

The two different types of addresses _are_ defined as top-level complex
types. `Address` in the `Veterinarian` element is defined by the
`InternationalAddress` complex type. This distinction allows for the
possibility that a veterinarian could have a practice address in another
country but also be licensed and accredited in the US.

```xml
<xs:element name = "Veterinarian">
  <xs:complexType>
    <xs:sequence>
      <xs:element name = "Person" minOccurs = "1" maxOccurs = "1">
        <xs:complexType>
          <xs:sequence>
            <xs:choice>
              <xs:element name = "NameParts">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name = "BusinessName" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
                    <xs:element name = "FirstName" type = "xs:string" minOccurs = "1" maxOccurs = "1" />
                    <xs:element name = "MiddleName" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
                    <xs:element name = "LastName" type = "xs:string" minOccurs = "1" maxOccurs = "1" />
                    <xs:element name = "OtherName" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name = "Name" type = "xs:string" minOccurs = "1" maxOccurs = "1" />
            </xs:choice>
            <xs:element ref = "Phone" minOccurs = "0" maxOccurs = "unbounded" />
            <xs:element ref = "InternationalPhone" minOccurs = "0" maxOccurs = "unbounded" />
            <xs:element ref = "Email" minOccurs = "0" maxOccurs = "unbounded" />
          </xs:sequence>
        </xs:complexType>
      </xs:element>
      <xs:element name = "Address" type = "InternationalAddress" minOccurs = "0" maxOccurs = "1" />
    </xs:sequence> . . .
  </xs:complexType>
</xs:element>
```

```xml
<Veterinarian LicenseState=" SC " LicenseNumber=" 2403 " NationalAccreditationNumber=" 001234 ">
  <Person>
    <NameParts>
      <FirstName> Vinnie </FirstName>
      <LastName> Vet </LastName>
    </NameParts>
    <Phone Number=" 8761239876 "/>
    <Email Address=" Vinnie@vvet.com "/>
  </Person>
  <Address>
    <Line1> 123 1st Ave </Line1>
    <Town> Sometoen </Town>
    <County> FairCounty </County>
    <State> NY </State>
    <ZIP> 36002 </ZIP>
    <Country> USA </Country>
    <GeoPoint Latitude="9.227" Longitude="-17.35"/>
  </Address>
</Veterinarian>
```

### MovementPurposes

The `MovementPurposes` element is a fairly simple—but still called a
`xs:complexType` in XML—container for a list of individual `MovementPurpose`
elements. The list itself must exist once and only once. Within it may
be zero to any number of individual purposes. This is one example of
something you will notice throughout the schema. This is one of those
cases where the workgroup knew it could not account for all possible
reasons for movement so it also allows an “other” value. Then an
`OtherReason` element must provide the reason as a simple string.

So why do we have `MovementPurposes` that may be empty but, later in the
schema, Attachment that may have zero or many copies? Just another
stylistic fluke. So, “Sorry” to new implementers and, “You are welcome”
to those who have been around from the beginning and don’t want to
change just for consistency.

```xml
<xs:element name = "eCVI" >
  . . .
  <xs:complexType>
    <xs:sequence>
      . . .
      <xs:element ref = "MovementPurposes" minOccurs = "1" maxOccurs = "1" />
      . . .
```

```xml
<eCVI . . . >
  . . .
  <MovementPurposes>
    <MovementPurpose> Competition </MovementPurpose>
    <MovementPurpose> Other </MovementPurpose>
    <OtherReason> Abduction by aliens </OtherReason>
  <MovementPurposes>
. . .
```

### USAddress

What most Americans think of as simply “address” is a street address in
the US. It includes the street address (one or two lines), city (town),
state, and zip code. A zip code may be either five or nine digits. The
standard also allows for county. It also allows for a country code that
because this is a US address must be “USA.”

The `USAddress` type includes child elements: `Line1`, `Line2`, `Town`, `County`,
`State`, `Zip`, `Country`, and `GeoPoint`. If a `GeoPoint` element is included, it
must have `Latitude` and `Longitude` attributes. These are defined as
floating-point numbers with the appropriate allowed values for decimal
degrees.

Of all these elements, only the `State` element is required. This is
to allow maximum utility of the standard. But be aware that the full
requiredness is determined outside the schema by state and federal
traceability rules.

`Address` is restricted to `USAddress` in only two places in the schema.
Premises must be in the US based on the scope of the standard. And if
testing is performed “in the field” the location of testing must be in
the US.

```xml
<xs:complexType name = "USAddress">
  <xs:sequence>
    <xs:element name = "Line1" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
    <xs:element name = "Line2" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
    <xs:element name = "Town" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
    <xs:element name = "County" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
    <xs:element name = "State" type = "StateCodeType" minOccurs = "1" maxOccurs = "1" />
    <xs:element name = "ZIP" minOccurs = "0" maxOccurs = "1">
      <xs:simpleType>
        <xs:restriction base = "xs:string">
          <xs:pattern value = "\\d{ 5 }" />
          <xs:pattern value = "\\d{ 5 }-\\d{ 4 }" />
        </xs:restriction>
      </xs:simpleType>
    </xs:element>
    <xs:element name = "Country" minOccurs = "0" maxOccurs = "1">
      <xs:simpleType>
        <xs:restriction base = "xs:string">
          <xs:enumeration value = "USA" />
        </xs:restriction>
      </xs:simpleType>
    </xs:element>
    <xs:element name = "GeoPoint" minOccurs = "0" maxOccurs = "1">
      <xs:complexType>
        <xs:attribute name = "Latitude" type = "LatitudeType" use = "required" />
        <xs:attribute name = "Longitude" type = "LongitudeType" use = "required" />
      </xs:complexType>
    </xs:element>
  </xs:sequence>
</xs:complexType>
```

```xml
<Address>
  <Line1> 123 1st Ave </Line1>
  <Town> Someto w n </Town>
  <County> FairCounty </County>
  <State> NY </State>
  <ZIP> 36002 </ZIP>
  <Country> USA </Country>
  <GeoPoint Latitude="9.227" Longitude="-17.35"/>
</Address>
```

### InternationalAddress

I presented `USAddress` first so that I can discuss `InternationalAddress`
by comparison. The structure is identical by design. This supports
common usage and database structures that can contain either. Some of
the child element names are used analogously. For example, the major
political subdivision of some countries is called “county” rather than
“state” but would belong in the analogous State element. And the
additional level is sometimes called “parish” rather than “county.”
These might have been more correctly named “Major Political Subdivision”
and “Additional Political Subdivision” but because the vast majority of
addresses will be US addresses, the naming follows the most common US
pattern. The same logic applies to the “postal code” element that
retains the element name `ZIP`.

The defining difference between `InternationalAddress` and `USAddress` is
the looser definitions of `ZIP` and `CountryCode` both of which are simple
strings. `CountryCode` is any three-character string. (This could have
been a huge RegEx of all known country codes but would have created
maintenance issues.) `ZIP` can be any string because different countries
use very different formatting of postal codes.

It is interesting that the US address vs. international address issue is
very similar to that of natural person vs. legal person. Address is
defined in slightly different ways in different places by using defined
complex types. Person’s definition is changed by overwriting it in the
`Veterinarian` definition rather than by defining two distinct complex
types. The effect is identical in each case. Once again this can be
partly explained by the time over which development has taken place and
the workgroup’s preference for leaving things alone. Either of these
could be refactored to match in the schema without affecting the
structure of valid documents. Because the only natural person is in
Veterinarian, it made some sense to just do it locally rather than
define and reference two different complex types. USAddress is used in
two different elements so overwriting in-line would have duplicated
code. If working “from scratch” natural person and legal person would
probably be defined types similar to the two address types.

```xml
<xs:complexType name = "InternationalAddress">
  <xs:sequence>
    <xs:element name = "Line1" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
    <xs:element name = "Line2" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
    <xs:element name = "Town" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
    <xs:element name = "County" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
    <xs:element name = "State" type = "xs:string" minOccurs = " 0 " maxOccurs = "1" / >
      <xs:element name = "ZIP" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
      <xs:element name = "Country" minOccurs = "0" maxOccurs = "1">
        <xs:simpleType>
          <xs:restriction base = "xs:string">
            <xs:length value = "3" />
          </xs:restriction>
        </xs:simpleType>
      </xs:element>
      <xs:element name = "GeoPoint" minOccurs = "0" maxOccurs = "1">
        <xs:complexType>
          <xs:attribute name = "Latitude" type = "LatitudeType" use = "required" />
          <xs:attribute name = "Longitude" type = "LongitudeType" use = "required" />
        </xs:complexType>
      </xs:element>
    </xs:sequence>
  </xs:complexType>
```

```xml
<Address>
  <Line1> Kastanievej 15 </Line1>
  <Town> SKANDERBORG </Town>
  <State> DK </State>
  <ZIP> 8660 </ZIP>
  <Country> D NK </Country>
</Address>
```

Note one odd detail. In making this made-up example, in Denmark
addresses do not include a “state or province” element. Because “State”
was required in version 3.0, we used the “DK” country code that is
sometimes included in their postal codes. Sometimes implementations
dealing with these kinds of “edge cases” have to construct work-arounds
like this. The standard does not account for absolutely every possible
case. Version 3.1 changed this to make `State` optional in
`InternationalAddress`, but not `USAddress`. Leaving `State` out of an
`InternationalAddress` is meant only for countries that do not have
political subdivisions and should be extremely rare.

### Origin, Destination, PremType

Because the data structures of Origin and Destination are identical,
they are defined as very thin elements with all the details in the
defined complex type `PremType`. The `PremType` is a complex type containing
the basic premises identification as well as statuses that apply at the
premises level.

With premises identification we get right back into politics. The first
child element is `PremId` which is a seven-character national premises
identification number (PIN), or a six- or eight-character state issued
location identifier (LID). This is defined as a string of six to eight
capital letters or digits. Proper validation of PINs is much more
complex, including a check digit that is valuable for catching
typographical errors. The need to support several different official
formats limits more strict validation here. And because even the LID
compromise did not eliminate the political objection to a nationally
unique place identifier, this element is optional. It can occur zero or
one time. The check digit algorithm is covered in Appendix A.

The `PremName` element is a simple string that is also optional, zero or
one occurrences.

`Address` is a required instance of the `USAddress` type. While it is not
enforceable via schema language, traceability rules require that this be
a physical street address, not a postal route or box number. One and
only one Address is required.

A premises may have any number of disease control program statuses
either from its location in a state or other defined area that has a
status or by participation in a herd status program. These are covered
in more detail in the next two sections.

Finally, the premises may have any number of `Person` elements. These are
the normal “legal person” definition of `Person` so they may be businesses
or actual people such as the owner of the premises, a market company, or
other responsible entity. The `Person` records here relate to the _place_
from which the animals move. If the consignor or consignee has a
different contact address that information would belong in a separate
element, `Consignor` or `Consignee`.

```xml
<xs:complexType name = "PremType">
  <xs:sequence>
    <xs:element name = "PremId" type = "PremIdType" minOccurs = "0" maxOccurs = "1" />
    <xs:element name = "PremName" type = "xs:string" minOccurs = "0" maxOccurs = "1" />
    <xs:element name = "Address" type = "USAddress" minOccurs = "1" maxOccurs = "1" />
    <xs:element ref = "StateZoneOrAreaStatus" minOccurs = "0" maxOccurs = "unbounded" />
    <xs:element ref = "HerdOrFlockStatus" minOccurs = "0" maxOccurs = "unbounded" />
    <xs:element ref = "Person" minOccurs = "0" maxOccurs = "unbounded" />
  </xs:sequence>
</xs:complexType>
```

```xml
<Origin>
<PremId> 003EZUN </PremId>
<PremName> SomeFarm Home Place </PremName>
<Address>
  <Line1> 123 Real Road </Line1>
  <Town> Sometown </Town>
  <State> MI </State>
  <ZIP> 00634-7353 </ZIP>
  <Country> USA </Country>
</Address>
<StateZoneOrAreaStatus>
  <TuberculosisStateOrZoneStatus Status = "Modified Accredited Advanced State or Zone (MAA)"/>
</StateZoneOrAreaStatus>
<HerdOrFlockStatus Disease = " Brucellosis " HerdOrFlockID = "FS7bwxJTj" Status = " Free "/>
<Person>
  <Name> Polly PremOwner </Name>
  <Phone Number = "7338812262" />
  <Email Address = " Polly@Somefarm.com " />
</Person>
</Origin>
```

### StateZoneOrAreaStatus

This `StateZoneOrAreaStatus` element is a child element of `PremType` and
can repeat. Each instance contains one child element that may be for TB,
Brucellosis, or Other. Each of these is an empty XML element whose name
tells the type of status and whose one attribute Status is pulled from a
list of disease program specific status titles.

`BrucellosisStateOrAreaStatus` can have values: “Free”, “Class A”, “Class
B”, “Class C”, or “GYA, DSA (Class A)”.

`TuberculosisStateOrZoneStatus` can have values: “Free”, “Modified
Accredited Advanced State or Zone (MAA)”, “Modified Accredited State or
Zone (MA)”, or “Non Accredited State or Zone (NA)”.

One note of caution here. While the status values above are very
human-readable, they are actually _codes_. That is, they must have
_exactly_ the characters listed in the schema to validate. This allows
receiving systems to map directly to their database representation of
these statuses rather than require human interpretation.

There is also an element for anything else. `OtherStateOrZoneStatus` is
one of those future-proofing “other” categories. As with the other,
“other” cases, this must not be used to carry TB or Brucellosis
statuses that can go into the above structures. Unlike the first two,
this one has two attributes, the first is a `Disease` as ordinary text and
the second is the `Status`, which in this case is another ordinary string.

```xml
<xs:element name = "StateZoneOrAreaStatus">
  <xs:complexType>
    <xs:sequence>
      <xs:choice>
        <xs:element ref = "BrucellosisStateOrAreaStatus" minOccurs = "1" maxOccurs = "1" />
        <xs:element ref = "TuberculosisStateOrZoneStatus" minOccurs = "1" maxOccurs = "1" />
        <xs:element ref = "OtherStateOrZoneStatus" minOccurs = "1" maxOccurs = "1" />
      </xs:choice>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

### HerdOrFlockStatus

A herd or flock status is more complicated in real-life but simpler here
because the number of variations is beyond enumerating the way the
standard does with state and zone statuses. The `HerdOrFlockStatus`
element can repeat any number of times in a `PremType` element. Each
instance is an empty element with up to three attributes. `Disease` is the
disease covered by the status as a required non-empty string. Many herd
and flock certification programs issue program specific identifiers for
the covered entity. The attribute `HerdOrFlockID` is an optional simple
string to hold this identifier, if any. Some certification programs have
distinct status levels. The status or status level goes in the optional
simple string attribute Status.

```xml
<xs:element name = "HerdOrFlockStatus">
  <xs:complexType>
    <xs:attribute name = "Disease" type = "nonNullString" use = "required" / >
      <xs:attribute name = "HerdOrFlockID" type = "xs:string" use = "optional" / >
        <xs:attribute name = "Status" type = "xs:string" use = "optional" / ></xs:complexType>
      </xs:element>
```

```xml
<Origin>
  <PremId> 003EZUN </PremId>
  <PremName> SomeFarm Home Place </PremName>
  <Address>
    <Line1> 123 Real Road </Line1><Town> Sometown </Town><State> MI </State><ZIP> 00634-7353 </ZIP><Country> USA </Country>
  <Address>
  <StateZoneOrAreaStatus>
    <TuberculosisStateOrZoneStatus Status = "Modified Accredited Advanced State or Zone (MAA)" />
  </StateZoneOrAreaStatus>
  <StateZoneOrAreaStatus>
    <Other StateOrZoneStatus Disease = " Nose and Tail disease " Status = " Free " />
  </StateZoneOrAreaStatus>
  <HerdOrFlockStatus Disease = " Brucellosis " HerdOrFlockID = "FS7bwxJTj" Status = " Free " />
  <Person>
    <Name> Polly PremOwner </Name>
    <Phone Number = "7338812262" />
    <Email Address = " Polly@Somefarm.com " />
  </Person>
</Origin>
```

### Consignor, Consignee, ContactType

`ContactType` is used to define both `Consignor` and `Consignee`. This
consists of an `Address` element of the international type of address. The
`Address` is optional. It is followed by one or more `Person` elements of
the “legal person” variety. Why are these elements in this order? The
only reason is to parallel the related structure of the `PremType`. For
the `ContactType` definition of `Address` a postal box or route number is
acceptable because this is designed for making contact with a
responsible person rather than to locate animals.

```xml
<xs:complexType name = "ContactType" >
  <xs:sequence>
    <xs:element name = "Address" type = "InternationalAddress" minOccurs = "0" maxOccurs = "1" />
    <xs:element ref = "Person" minOccurs = "1" maxOccurs = "unbounded" /> </xs:sequence>
  </xs:complexType>
```

```xml
<Consignor>
  <Address>
    <Line1> Kastanievej 15 </Line1>
    <Town> SKANDERBORG </Town>
    <State> DK </State>
    <ZIP> 8660 </ZIP>
    <Country> DNK </Country>
  </Address>
  <Person>
    <NameParts>
      <BusinessName> Shipper Inc. </BusinessName>
      <FirstName> Sally </FirstName>
      <MiddleName> Q </MiddleName>
      <LastName> Shipper </LastName>
      <OtherName> von Shiple </OtherName>
    </NameParts>
    <Phone Number = "2290040270" Type = "Cellphone" />
    <InternationalPhone Number = "39123445" Comment = "Intergalactic hyperphone" />
    <Email Address = "Sally@shipper.com" />
    <Email Address = "Shipping@shipper.com" />
  </Person></Consignor>
```

### Animal

After the origin and destination information, the most important
information in a CVI is that which identifies and describes the animals,
or animal products that have been inspected. The `Animal` element gets
complicated, but we can simplify it by taking one layer of information
at a time and then looking at each in more detail. Remember that an eCVI
must contain one of the three element types (`Animal`, `GroupLot`, or
`Product`) but can have as many as necessary in any order.

The `Animal` element has attributes for `Age`, `Breed`, `Sex`, and
`InspectionDate`. Of these, only the `InspectionDate` is required.

`Age` is a complicated simple type! This includes both the numeric value
and units in one string defined by a pair of complicated regular
expressions. Receiving systems have some intricate parsing to do. One
option is to include the date of birth in a format that matches the
`xs:date` (YYYY-MM-DD). The other option is to include age in days, weeks,
months, or years. The abbreviations for these are: “d”, “wk”, “mo”, and
“a”. Lower case “a” is the international abbreviation for year (annum,
I guess). A space is optional. So, a six-week-old animal would have
`Age=”6wk”` or `Age=“6 wk”` for this attribute.

`Breed` is a simple `xs:string`. This is _not_ for the official taxonomy of
the animal, but for any additional breed specification. Properly
distinguishing those is a challenge for eCVI developers to work out in
the user interface. If official three-letter breed codes exist, those
_should_ be used, but this field allows flexibility.

`Sex` is a pair of attributes. The `Sex` attribute itself has an enumerated
value set: “Female”, “Male”, “Spayed Female”, “Neutered Male”, “True
Hermaphrodite”, “Gender Unknown” and “Other”. There is that nasty
“other” again. The working group discussed a large list of other
weird intersex categories and eventually decided that the confusion
presented by an attempt at an exhaustive list would add more confusion
than it would shed light. If “Other” is selected, the best possible
description of the sex goes in `SexDetail` as a simple `xs:string`. And
again, schema language cannot catch a violation of this usage rule but
that is the intent.

`InspectionDate` is a required `xs:date` attribute that, as the name
implies, is the date that this animal was inspected. Most people think
of the inspection date as an attribute of the CVI rather than each
individual animal and in most cases this date will be the same for all
animals on an eCVI. Making this an attribute of each animal simplifies
those case where inspection takes more than one day. The structure
remains unchanged and just the animals inspected on the second, third
etc. days get different inspection dates. Computers don’t mind this kind
of duplication. User interfaces certainly don’t need to force
veterinarians to re-enter the date for each animal. All this can take
place behind the scenes. And the added space, which seems to bother some
people, really is trivial. XML is a very space conservative format
compared to any binary format such as PDF, Word, Excel, etc. anyway.

The `Animal` element then has child elements for `Species` (official
taxonomy), `AnimalTags` (identifiers), `Tests`, `Vaccinations`, and for
`Statements` and `MiscAttributes` that are animal-specific. We will look at
these in detail a little later. Version 3.1 added an element for
`CountryOfBirth`.

```xml
<xs:element name = "Animal">
  <xs:complexType>
    <xs:sequence>
      <xs:choice>
        <xs:element ref = "SpeciesCode" minOccurs = "1" maxOccurs = "1" />
        <xs:element ref = "SpeciesOther" minOccurs = "1" maxOccurs = "1" />
      </xs:choice>
      <xs:element ref = "AnimalTags" minOccurs = "1" maxOccurs = "1" />
      <xs:element ref = "Test" minOccurs = "0" maxOccurs = "unbounded" />
      <xs:element ref = "Vaccination" minOccurs = "0" maxOccurs = "unbounded" />
      <xs:element ref = "CountryOfBirth" minOccurs = "0" maxOccurs = "1" />
      <xs:element ref = "Statements" minOccurs = "0" maxOccurs = "1" />
      <xs:element ref = "MiscAttribute" minOccurs = "0" maxOccurs = "unbounded" />
    </xs:sequence>
    <xs:attribute name = "Age" type = "AgeType" use = "optional" />
    <xs:attribute name = "Breed" type = "xs:string" use = "optional" />
    <xs:attribute name = "Sex" type = "SexType" use = "optional" />
    <xs:attribute name = "SexDetail" type = "xs:string" use = "optional" />
    <xs:attribute name = "InspectionDate" type = "xs:date" use = "required" />
  </xs:complexType>
</xs:element>
```

```xml
<Animal Age = "2023-02-30" Breed = "Black" Sex = "Female" InspectionDate = "2024-02-11">
  <SpeciesCode . . . />
  <AnimalTags> . . . </AnimalTags>
  <Test . . . > . . . </Test>
  <Vaccination . . . > . . . </Vaccination>
  <Statements> This one animal has something different from the others. </Statements>
</Animal>
```

### GroupLot

`GroupLot` is the element used for all animals that _do not_ have
individual official identification on the CVI. The federal rules for
what do and do not require official identification on CVIs is way more
complicated that we can cover here. See 9CFR86 and other official
sources, including InterstateLivestock.com for help. Probably the most
important part of this element is the `Description` attribute that is
required and must include the exemption to individual official animal
identifiers in a simple string. Other than the species, everything else
is optional, not because it is not needed but because of variation
between what is needed for various types of groups.

`Quantity` is a floating-point number, which makes no sense at all for the
usual case of a truck load of calves, etc. Those would always be whole
numbers. Other types of groups might be measured in tons, etc. If the
quantity is anything other than a simple number of animals, the
attribute Unit should be filled in with a simple `xs:string` stating the
units. Schema language does not enforce this requirement, but it would
make no sense to send something like “14.5” without telling what that
measured.

The `Age` attribute is the `AgeType` the same as we saw in `Animal`. Because this applies to
the entire group, if all animals on the CVI are not the same age, to the
precision in the age units, then multiple `GroupLot` elements will be
needed. This can often be handled by using the inequality symbol for
less than for example `“&lt;6mo”` for less than 6-month-old calves. There
is one of those XML entities we mentioned earlier. This of course should
extract as `“<6mo”` by the time a human reads it.

The `Breed` attribute is the same as in the `Animal` element. This is
another place where multiple groups may be required if members are of
different breeds and those are or need to be included on the CVI.

`Sex` is defined with a slightly different variation from `SexType` in the
`Animal` element. `GroupSexType` adds the value “Mixed Group.” Before using
this value, veterinarians should be certain that regulations do not vary
by sex. There is nothing the schema can do and little the eCVI developer
can do to prevent mixing groups with different regulatory requirements.
But a good system will make it clear that they can and should be split
if necessary.

The same rule applies to use of “Other” and `SexDetail` as applied in the
Animal element.

I have already discussed the `Description` attribute. This can duplicate
some but must not replace information from structured child elements and
attributes. This is the human-readable description and must include the
reason these are not in individual `Animal` elements. So, you might have
“Load of 25 hereford calves less than 6 months of age” that duplicates
species, breed, quantity, and age, but is needed to explain the
exemption.

The `InspectionDate` attribute is listed as optional in the schema. This
will be required in the real-world in most if not all cases.

The list of child elements is almost the same as in the `Animal` element.

The first element is the same choice of `SpeciesCode` or `SpeciesOther` as
in `Animal` with the same rules about use of “other.” It is important to
note that just because description may include the species in text form,
the structured species code is still required.

`GroupLotID` is a simple `xs:string` element that may occur zero to many
times. So, at the data level, this is simple. At the real-world level,
it gets much more interesting. USDA regulations for Group Identification
Number (GIN) get very complicated. Other group movements are allowed for
specific species and identifier types. In some cases, groups may even be
combined. That is when multiple `GroupLotID`s might be needed. This is
_not_ there to allow identification of a group by listing individual
animal IDs. Those must go in individual Animal elements.

The `Test`, `Vaccination`, `Statements`, and `MiscAttribute` elements are all
the same as in the `Animal` element with the added requirement that any
values here must apply to all animals in the group or to the group as a
whole. Accessions, tests, and vaccinations will be covered in more
detail in a later section. Version 3.1 added an element for
`CountryOfBirth`.

```xml
<xs:element name = "GroupLot">
  <xs:complexType>
    <xs:sequence>
      <xs:choice>
        <xs:element ref = "SpeciesCode" />
        <xs:element ref = "SpeciesOther" />
      </xs:choice>
      <xs:element name = "GroupLotID" type = "xs:string" minOccurs = "0" maxOccurs = "unbounded" />
      <xs:element ref = "Test" minOccurs = "0" maxOccurs = "unbounded" />
      <xs:element ref = "Vaccination" minOccurs = "0" maxOccurs = "unbounded" />
      <xs:element ref = "CountryOfBirth" minOccurs = "0" maxOccurs = "1" />
      <xs:element ref = "Statements" minOccurs = "0" maxOccurs = "1" />
      <xs:element ref = "MiscAttribute" minOccurs = "0" maxOccurs = "unbounded" />
    </xs:sequence>
    <xs:attribute name = "Quantity" type = "xs:float" use = "optional" />
    <xs:attribute name = "Unit" type = "xs:string" use = "optional" default = "Number" />
    <xs:attribute name = "Age" type = "AgeType" use = "optional" />
    <xs:attribute name = "Breed" type = "xs:string" use = "optional" />
    <xs:attribute name = "Sex" type = "GroupSexType" use = "optional" />
    <xs:attribute name = "SexDetail" type = "xs:string" use = "optional" />
    <xs:attribute name = "Description" type = "nonNullString" use = "required" />
    <xs:attribute name = "InspectionDate" type = "xs:date" use = "optional" />
  </xs:complexType>
</xs:element>
```

```xml
<GroupLot Quantity = "50" Age = "5mo" Breed = "AN" Sex = "Mixed Group" Description = "Beef calves under six months of age">
  <SpeciesCode Code = "BEF" />
  <GroupLotID> 1234 </GroupLotID>
  <Test . . . > . . . </Test>
  <Vaccination . . . > . . . </Vaccination>
  <Statements> No cases of nose and tail disease in this or adjacent counties. </Statements>
</GroupLot>
```

### Product

A more recent addition to the eCVI standard schema is the ability to
send animal-derived products that require veterinary inspection. The
list of covered products is: “Embryos”, “Hatching Eggs”, “Liquid Egg
(Non-Pasteurized)”, “Liquid Egg (Pasteurized)”, “Milk (Pasteurized)”,
“Milk (Raw)”, “Mohair/Cashmere”, “Shell Eggs (Nest Run)”, “Shell Eggs
(Washed/Sanitized)”, “Shells/Inedible Egg Product”, “Semen”, and “Wool”.

Looking at the schema definition of the `Product` element, you will find
it very similar to `GroupLot`. That is not an accident. It was done in the
spirit of supporting re-use of as much development infrastructure as
possible. Certain attributes and elements may make no sense combined
with some commodity types but be essential in others.

The only always-required parts of the `Product` element are the
`ProductType` and `Description` attributes, and the `SpeciesCode` or
`SpeciesOther` element. `Quantity` and `Unit` are optional but very likely
essential. The floating-point number value for `Quantity` makes more sense
in this context than it did in `GroupLot`.

```xml
<xs:element name = "Product">
  <xs:complexType>
    <xs:sequence>
      <xs:choice>
        <xs:element ref = "SpeciesCode" />
        <xs:element ref = "SpeciesOther" />
      </xs:choice>
      <xs:element name = "ProductID" type = "xs:string" minOccurs = "0" maxOccurs = "unbounded" />
      <xs:element ref = "Test" minOccurs = "0" maxOccurs = "unbounded" />
      <xs:element ref = "Vaccination" minOccurs = "0" maxOccurs = "unbounded" />
      <xs:element ref = "Statements" minOccurs = "0" maxOccurs = "1" />
      <xs:element ref = "MiscAttribute" minOccurs = "0" maxOccurs = "unbounded" />
    </xs:sequence>
    <xs:attribute name = "ProductType" type = "CommodityType" use = "required" />
    <xs:attribute name = "Quantity" type = "xs:float" use = "optional" />
    <xs:attribute name = "Unit" type = "xs:string" use = "optional" default = "Number" />
    <xs:attribute name = "Breed" type = "xs:string" use = "optional" />
    <xs:attribute name = "Description" type = "nonNullString" use = "required" />
    <xs:attribute name = "InspectionDate" type = "xs:date" use = "optional" />
  </xs:complexType>
</xs:element>
```

```xml
<Product ProductType = "Semen" Quantity = "25" Unit = "Straws" Breed = "AN" Description = "Purebred Frozen Yak Semen" InspectionDate = "2024-02-05">
  <SpeciesOther Code="OTH" Text="Yak"/>
  <ProductID>HCELE5Nr3</ProductID>
  <Test . . .>
    . . .
  </Test>
  <Statements>Has met all Yak semen requirements</Statements>
  <MiscAttribute Name="Liquid Nitrogen Qty" Value="0.75l"/>
</Product>
```

## Closer Look at Some Complex Child Elements

Now we dig into some of the more complex and or confusing elements that
make up some of the detailed information carried in the eCVI.

### SpeciesCode and SpeciesOther

`SpeciesCode` and `SpeciesOther` differ only in the requiredness of the two
attributes `Code` and `Text`. For `SpeciesCode` the code is required and may
have a text description, while in `SpeciesOther` the code defaults to
“OTH” and it is the text description of the unusual species that is
required. Inclusion of the optional half in each case is most useful
during development to clarify meaning when read by humans on either end
of the transaction.

```xml
<xs:element name="SpeciesCode">
  <xs:complexType>
    <xs:attribute name="Code" type="SpeciesCodes" use="required" />
    <xs:attribute name="Text" type="xs:string" use="optional" />
  </xs:complexType>
</xs:element>
<xs:element name="SpeciesOther">
  <xs:complexType>
    <xs:attribute name="Code" default="OTH" use="optional" />
    <xs:attribute name="Text" type="nonNullString" use="required" />
  </xs:complexType>
</xs:element>
```

```xml
<Animal ...>
  <SpeciesCode Code="BEF"/>
  ...
</Animal>

<Animal ...>
  <SpeciesCode Code="DAI" Text="Dairy Cattle"/>
  ...
</Animal>

<Animal ...>
  <SpeciesOther Text="Aardvark"/>
  ...
</Animal>

<Animal ...>
  <SpeciesOther Code="OTH" Text="Aardvark"/>
  ...
</Animal>
```

### AnimalTags

As the default animal disease traceability documentation in the US, an
eCVI must effectively identify the animals represented in it. Animal
identification is another topic that has undergone decades of political
debate and discussion. The eCVI standard cannot impose identification
requirements beyond what official authorities have done, but it tries to
ensure the greatest possible compliance with those rules, and correct
recording of those identifiers. To this end, the AnimalTags element
includes a lot of very specific detail.

`AnimalTags` is simply a list of one or more elements that represent
various animal identification types. Note that the name “animal tags” is
overly specific because some of these are not in the form of an eartag,
etc. Most, but not all, of these child elements follow the same pattern
of a single attribute named `Number` that contains the identifier, numeric
or not. The name of the child element indicates the type of identifier,
and the definition specifies a format pattern (RegEx) that identifiers
of that type must match. Some legally official animal identification
does not consist of a string of characters that can be called a
“number.” These include brand images, equine descriptions, and
equine photographs. Because not all official identifier types lend
themselves to verifiable pattern matching, there is also an element for
`OtherOfficialID` that includes a list of `TagTypes` to be specified in the
`Type` attribute along with the `Number` attribute.

Version 3.0.1 added an `InternationAIN` tag type. These are official RFID
tags with the first three digits being the ISO country code where they
were issued.

Version 3.1 adds a tag type for a special kind of RFID tag specifically
for foreign born animals imported with official ID from their country of
origin but that have lost their tags. “Country of origin” is regulatory
jargon for the country the animal was born in, so the standard uses more
plain English “country of birth” in the element that identifies the
birth country. These tags are normal manufacturer RFID tags but with
different color and labeling. `OfficialIntRFID` numbers cannot be
distinguished from `MfrRFID` except by the element name so their pattern
for validation is the same as `MfrRFID`. It uses the same `MfrRFIDType`
definition.

XML schema language does not provide a means to prevent putting a
specifically defined animal identifier type into `OtherOfficialID` with
type of “Other” but doing so keeps the schema from being able to do its
data quality job. If the user selects “AIN” as the identifier type and
enters only 14 digits, the schema can catch the error. The same in
`OtherOfficialID` would remain uncaught and the veterinarian might well
get a nasty letter from their state animal health official.

The formally defined identifier types are: `AIN`, `MfrRFID`,
`OfficialIntRFID`, `NUES9`, `NUES8`, `ManagementID`, `EquineDescription`,
`EquinePhotographs`, and `BrandImage`. The regular expressions that define
the first five of these will be explained in the later section on
detailed patterns. The last three need a little discussion here.

`EquineDescription` consists of two xs:string attributes. The first of
these, Name, is optional and contains the registered name of the horse,
if any. But by rule, the registered name is only official if accompanied
by a description, so the second attribute, `Description`, is required. In
the real-world there are very specific requirements for an equine
description to be sufficient identification. Those are beyond the
capability of schema language to enforce, so this attribute is a simple
string.

```xml
<xs:element name="AnimalTags">
  <xs:complexType>
    <xs:sequence minOccurs="1" maxOccurs="unbounded">
      <xs:choice>
        <xs:element ref="AIN"/>
        <xs:element ref="MfrRFID"/>
        <xs:element ref="InternationalAIN"/>
        <xs:element ref="OfficialIntRFID"/>
        <xs:element ref="NUES9"/>
        <xs:element ref="NUES8"/>
        <xs:element ref="OtherOfficialID"/>
        <xs:element ref="ManagementID"/>
        <xs:element ref="BrandImage"/>
        <xs:element ref="EquineDescription"/>
        <xs:element ref="EquinePhotographs"/>
      </xs:choice>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

The last two defined identifier types I discuss, `BrandImage` and
`EquinePhotographs`, are related in that they both involve binary images.
Here we will begin discussion of how binary data are handled and then
cover the Binary element in more detail later.

Sometimes the data in an element is very large or needs to be included
in multiple locations. In these cases, the content can be included by
reference to a different location in the document using special XML
attribute types `ID` and `IDREF`. An `ID` attribute must have a unique value
within the document, allowing for easy identification of specific
elements. An `IDREF` is used to reference the `ID` value of another element.
The eCVI schema uses this for attachments and other large binary
content. They are also used for laboratory accessions that may apply to
many of tests on many animals. Using `IDREF` avoids repeating the
accession details for each test.

`EquinePhotographs` is an element that consists of one to three `Photograph`
child elements. Each of these has two attributes: ImageRef and View.
ImageRef is an `xs:IDREF` identifier that references (links to) the binary
image itself near the end of the document. We will get into what this
identifier is later but for now just know that it must match the `ID` in
one and only one `Binary` element.

```xml
<xs:element name="EquinePhotographs">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="Photograph" minOccurs="1" maxOccurs="3">
        <xs:complexType>
          <xs:attribute name="ImageRef" type="xs:IDREF" use="required"/>
          <xs:attribute name="View" type="PhotoView" use="optional"/>
        </xs:complexType>
      </xs:element>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

The `View` attribute in `EquinePhotographs` is defined by the simple type `PhotoView` and can be “Left”, “Front”, or “Right”.

`BrandImage` is very similar except that it is just one element with two attributes. The `BrandImageRef` attribute works just like `ImageRef` in `Photograph`. It points to the binary representation of the brand. An additional attribute `Description` is optional.

```xml
<xs:element name="BrandImage">
  <xs:complexType>
    <xs:attribute name="BrandImageRef" type="xs:IDREF" use="required"/>
    <xs:attribute name="Description" type="xs:string" use="optional"/>
  </xs:complexType>
</xs:element>
```

This example `Animal` is a bit silly but shows three of the `AnimalTags` element types.

```xml
<Animal ...>
  ...
  <AnimalTags>
    <BrandImage BrandImageRef="ID002" Description="Double J Ranch Brand"/>
    <MfrRFID Number="914709631860503"/>
    <EquinePhotographs>
      <Photograph ImageRef="ID004" View="Left"/>
      <Photograph ImageRef="ID005" View="Front"/>
      <Photograph ImageRef="ID006" View="Right"/>
    </EquinePhotographs>
  </AnimalTags>
  ...
</Animal>
```

### Accession

Before I get into the technical details, I should note that in
veterinary medicine the term “accession” has taken on a slightly
different meaning than in human-medicine. The government’s United States
Core Data for Interoperability (USCDI) defines accession as, “The unique
identifier for a single instance of a specimen received by a laboratory
and its analysis.” In veterinary regulatory laboratories the full set of
samples that arrive from a testing event are normally assigned to a
single accession number. An exception to this is equine infectious
anemia (Coggins) testing where each sample gets a unique accession
number as in human medicine. So here, one accession may have one or many
tests on one or many animals.

We encounter `ID` and `IDREF` again in the way tests are handled. Because
complete information about a laboratory accession or field-testing event
can be more complicated than just a single string, we’d rather not have
to duplicate it multiple times for things like herd-testing for
Brucellosis or TB. Instead, the accession information goes in once and
is referenced by each test that is part of that accession.

The `Accessions` element at the root level of the `eCVI` element is just a
list of `Accession` elements. The `Accession` element contains an optional
`xs:Boolean` attribute called `InFieldTest` that is optional and defaults to
“false”. This information is redundant and left over as artifact from
when before the `Laboratory` and `Field` were defined separately.

The second, and often only, attribute is named `id`. Note the variance
from the normal naming convention. Oops. This carried over from very
early drafts and by the time it was noted fixing it would have created
work for existing implementers. This attribute is of type `xs:ID`. This is
a very special XML type. It is based on the XML type of “nonqualified
name” (`xs:NCName`) that gets complicated. And how “nonqualified” gets
abbreviated “NC” is just one of those mysteries. We can oversimplify
this to just that it must start with a letter and may contain letters,
digits, and some but not all, punctuation, etc. To be safe and easy,
stick to letters, digits, and maybe the underscore “\_” symbol. The
second requirement of xs:ID is that it must be unique within the XML
document.

The `Accession` element then contains one child element, either `Laboratory`
or `Field`. You may notice that this is one more level of nesting than is
probably necessary, and once again, that is because of gradual changes
over the years, not changing things that are ugly but not broken.

```xml
<xs:element name="Accession">
  <xs:complexType>
    <xs:sequence>
      <xs:choice>
        <xs:element ref="Laboratory" minOccurs="1" maxOccurs="1"/>
        <xs:element ref="Field" minOccurs="1" maxOccurs="1"/>
      </xs:choice>
    </xs:sequence>
    <xs:attribute name="InfieldTest" type="xs:boolean" use="optional" default="false"/>
    <xs:attribute name="id" type="xs:ID" use="required"/>
  </xs:complexType>
</xs:element>
```

```xml
<Accessions>
  <Accession InfieldTest="true" id="ID001">
    ...
  </Accession>
  <Accession InfieldTest="false" id="ID003">
    ...
  </Accession>
</Accessions>
```

The `Laboratory` element is what most veterinarians think of as an
“accession.” It contains two required attributes, `AccessionDate` is an
`xs:date` and `AccessionNumber` is a `nonNullString`, that is, it may not be
blank on just whitespace. There are very rare cases in which a
laboratory accession number does not exist or cannot be obtained. In
those cases, the string “Not Provided” may be entered. These should be
very rare.

There is also one required string element for the `LabName`. It may also
have elements for the lab’s PIN or LID as `PremId` and `Address`, which is
of the `InternationalAddress` type on the off chance that the lab is
outside the US.

```xml
<xs:element name="Laboratory">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="LabName" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="PremId" type="PremIdType" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Address" type="InternationalAddress" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute name="AccessionDate" type="xs:date" use="required"/>
    <xs:attribute name="AccessionNumber" type="nonNullString" use="required"/>
  </xs:complexType>
</xs:element>
```

```xml
<Accessions>
  <Accession InfieldTest="false" id="ID003">
    <Laboratory AccessionDate="2022-05-11" AccessionNumber="6">
      <LabName> Tests R Us Lab </LabName>
      <PremId> 16SVG2Y </PremId>
      <Address>
        <Line1> 123 Fifth Ave </Line1>
        <Line2> Suite 123 </Line2>
        <Town> Sometown </Town>
        <State> AL </State>
        <ZIP> 79474 </ZIP>
        <Country> USA </Country>
      </Address>
    </Laboratory>
  </Accession>
</Accessions>
```

The `Field` element is used to capture official testing that does not
require submission to a laboratory. The prototype of this is TB skin
testing. The only required part of the `Field` element is the
`AccessionDate` attribute. The two optional child elements are very
important, however, and should be included if available. They are the
`PremId`, and `Address`. `Address`, in this case, is limited to a US physical
address to define where the testing took place.

```xml
<xs:element name="Field">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="PremId" type="PremIdType" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Address" type="USAddress" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute name="AccessionDate" type="xs:date" use="required"/>
  </xs:complexType>
</xs:element>
```

```xml
<Accessions>
  <Accession InfieldTest="true" id="ID001">
    <Field AccessionDate="2021-12-29">
      <PremId>T6BO2H9</PremId>
      <Address>
        <Line1>123 Farm Drive</Line1>
        <Town>Sometown</Town>
        <County>Fairlain</County>
        <State>AL</State>
        <ZIP>79474</ZIP>
        <Country>USA</Country>
        <GeoPoint Latitude="24.487" Longitude="-17.197"/>
      </Address>
    </Field>
  </Accession>
</Accessions>
```

The `Accession` information having been entered at the eCVI level later
gets referenced in each `Animal` and `Test` to which it applies.

### Test

Diagnostic testing is probably the most challenging part of the CVI data
to standardize. On one hand, the standard must be flexible enough to
accommodate any type of test that is, or could be someday, required for
specific animal movements. On the other hand, the data must be
constrained to a consistent enough format that they can be used, on the
receiving end, to support regulatory decision making, trend analysis,
etc. And all that must be supportable with the information that the
veterinarian issuing the CVI will have available. Regulators would love
to know the precise laboratory methods used, but not every laboratory
includes those details on the reports available at the point of care
where the CVI is issued. The resulting standard is, of necessity, a
series of compromises.

The `Test` element begins with an `xs:IDREF` attribute named `AccessionRef`
that links to an `Accession`. This tiny detail is what makes the rest of
the compromise work. If all we communicate in the eCVI itself is a
vague, “Test for disease X was negative,” we may not have sufficient
precision about the testing to make important decisions. But information
contained in the `Accession` allows—admittedly future—interoperability
with laboratory information integration. For example, the National
Animal Health Laboratory Network (NAHLN) laboratory result messaging
protocol contains the same accession and animal information. Using the
eCVI data to query the NAHLN data would provide a much richer picture of
the diagnostic information. Realization of this possibility will require
much broader adoption of both standard protocols, but the foundation is
being laid in this one little, required, `AccessionRef` attribute.

The body of the `Test` element consists of a series of one or more `Result`
elements. The `Result` element has a `ResultName` attribute that is either
“RESULT” or “COMMENT” to distinguish deterministic result values from
interpretation comments. It then has one of three possible result types,
`ResultInteger`, `ResultString`, or `ResultFloat`. As these element names
imply, they have defined data types of `xs:integer`, `xs:string`, and
`xs:float`.

```xml
<xs:element name="Result" minOccurs="1" maxOccurs="unbounded">
  <xs:complexType>
    <xs:choice>
      <xs:element name="ResultInteger" type="xs:integer" minOccurs="0" maxOccurs="1"/>
      <xs:element name="ResultString" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="ResultFloat" type="xs:float" minOccurs="0" maxOccurs="1"/>
    </xs:choice>
    <xs:attribute name="ResultName" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:enumeration value="RESULT"/>
          <xs:enumeration value="COMMENT"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
  </xs:complexType>
</xs:element>
```

The last child element of `Test` is either `DiseaseCode` or DiseaseOther.
Here is that futureproofing again. The most common tests required are
included in the list of disease codes, but “other” is again available
because the list can never be absolutely complete. Both of these have
the same superficial structure, attributes for `Code` and `Text`. But in
`DiseaseCode`, the value for `Code` is constrained to a list of DiseaseType
while in `DiseaseOther` the code is fixed as “OTH”. And in `DiseaseCode`,
the `Text` is optional while in `DiseaseOther` it is required.

```xml
<xs:element name="Test">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="Result" .../>
      <xs:choice minOccurs="1" maxOccurs="unbounded">
        <xs:element ref="DiseaseCode"/>
        <xs:element ref="DiseaseOther"/>
      </xs:choice>
    </xs:sequence>
    <xs:attribute name="AccessionRef" type="xs:IDREF" use="required"/>
    <xs:attribute name="TestType" type="nonNullString" use="optional"/>
  </xs:complexType>
</xs:element>
```

As of version 3.0 of the eCVI standard, the diseases listed in
DiseaseType are: “Avian Influenza”, “Bovine Viral Diarrhea Virus”,
“Brucella abortus”, “Brucella canis”, “Brucella ovis”, “Brucella
suis”, “Caprine Arthritis and Encephalitis”, “Corynebacterium
pseudotuberculosis”, “Equine Infectious Anemia”, “Pseudorabies”,
“Rabies”, “Salmonella pullorum”, “Tritrichomonas foetus”, and
“Tuberculosis.” Note that these are again technically disease codes
rather than just names. That is, the _exact_ spelling and capitalization
is required. They were made human-readable rather than a standard
numeric or alphanumeric code to ease adoption. A wise developer will
implement this list in a way that will ease future additions or changes.

Version 3.1 adds an interesting twist. Note that the choice that allows
`DiseaseCode` or `DiseaseOther` can repeat (“unbounded”). This is to allow
for the rare cases in which one test can be used to detect more than one
disease. This is _not_ for the more common case of a “test” such as a
multiplex PCR that is actually a panel in a single tube. Those return
multiple different results. That case would show up as multiple tests in
one accession. One test with multiple diseases would have a result
similar to, “Negative for disease X, disease Y, and disease Z.”

To this point, this compromise structure says nothing of what type of
test was performed for the disease listed. That information may or may
not be immediately available to the issuing veterinarian. If it is, this
information can go back up in the opening tag of the Test element in an
optional attribute `TestType`. This is a `nonNullString` so if provided, it
may not be empty or all white space. It is otherwise unstructured text
to allow flexibility.

```xml
<Test AccessionRef="ID000" TestType=" FPA ">
  <Result ResultName="RESULT">
    <ResultInteger> 42 </ResultInteger>
  </Result>
  <Result ResultName="COMMENT">
    <ResultString> NEG </ResultString>
  </Result>
  <DiseaseCode Code="Brucella abortus"/>
</Test>
```

### Vaccination

Each `Animal` element can also have zero or more `Vaccination` elements.
There are similarities to the `Test` element but somewhat simpler. It has
two optional attributes and one child element.

The `VaccineType` attribute is a `nonNullString` similar to `TestType` to
allow that detail, if known. The `VaccineDate` is an `xs:date` attribute
that is also optional.

The choice of disease works the same way as in test, it may be
`DiseaseCode` or `DiseaseOther` with the same list of disease codes.

```xml
<xs:element name="Vaccination">
  <xs:complexType>
    <xs:choice minOccurs="1" maxOccurs="1">
      <xs:element ref="DiseaseCode"/>
      <xs:element ref="DiseaseOther"/>
    </xs:choice>
    <xs:attribute name="VaccineType" type="nonNullString" use="optional"/>
    <xs:attribute name="Date" type="xs:date" use="optional"/>
  </xs:complexType>
</xs:element>
```

```xml
<Vaccination VaccineType="RB51" Date="2021-03-08">
  <DiseaseCode Code="Brucella abortus"/>
</Vaccination>
```

### CountryOfBirth

Added in version 3.1, each `Animal` element can also have zero or one
`CountryOfBirth` element. If this element is omitted, it is implied to be
a US born animal as it will be in the vast majority of cases.

The `ForeignBorn` attribute is a `Boolean` value that will essentially
always be true but would be false if including this element for US born
animals, which is perfectly legal if it makes programming more
consistent.

The `CountryCode` is the three digit ISO country code. This is the same
value that makes up the first three digits of an ISO RFID number.
Because a major use-case for this element is those animals that once had
official ID from their country of origin, that value will most often be
readily available. Finding the ISO three letter alpha code might take a
trip to Wikipedia.

```xml
<xs:element name="CountryOfBirth">
  <xs:complexType>
    <xs:attribute name="ForeignBorn" type="xs:boolean" use="required"/>
    <xs:attribute name="CountryCode" type="ISOCountryCode" use="optional"/>
  </xs:complexType>
</xs:element>
```

```xml
<CountryOfBirth ForeignBorn="true" CountryCode="028"/>
```

### Attachment

After the list of `Animal`s, `GroupLot`s, and `Product`s as well as any
`Statements` that apply to the whole CVI, there can be any `Attachment`
elements. These would be things that get stapled to the paper CVI. In
the digital world they are “files” in some well-known format. These are
commonly PDF documents, Excel spreadsheets, etc. The data that would
when saved to disk be a “file” are encoded in a `Binary` element at the
very end of the eCVI. The `Attachment` element has an `xs:IDREF` attribute
`AttachmentRef` that contains the `ID` of the attachment `Binary` element. The
rest of the `Attachment` element is there to tell the receiver what the
attachment _is_.

The require attribute `DocType` tells the type of information in the
attachment. Allowed values are: “Scanned Paper CVI”, “Scanned Test
Chart”, “PDF CVI”, “PDF Test Chart”, and “Other.” The mime type of the
file itself will be included in the `Binary` element. There is a bit of
naming redundancy here. Because images of regulatory paperwork are so
commonly shared as PDF documents, the names PDF CVI and PDF Test Chart
are used. Any of these may typically be in Adobe PDF files. The
important difference is that the “Scanned…” types contain images of a
paper document while the “PDF…” types contain data rendered in PDF
format. The details of the PDF standard are complicated but beyond the
scope of this guide.

The `FileName` attribute is a required `nonNullString` that, as the name
implies, is the name of the file as it was or will be stored on disk. It
is best to use filenames that would be universally supported in any
operating system. Use letters and digits as well as very basic
punctuation such as “\_” and “-”. Avoid other special characters and
spaces. Pay attention to upper and lower case because some operating
systems ignore case while others distinguish between them. Many common
file formats have well accepted extensions such as “.pdf” or “.xlsx”
that can be helpful in addition to the mime type in the `Binary` element.

The optional `xs:string` attribute `Comment` can be very helpful, especially
in the case of “Other” `DocType` attachments.

```xml
<xs:element name="Attachment">
  <xs:complexType>
    <xs:attribute name="AttachmentRef" type="xs:IDREF" use="required"/>
    <xs:attribute name="DocType" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:enumeration value="Scanned Paper CVI"/>
          <xs:enumeration value="Scanned Test Chart"/>
          <xs:enumeration value="PDF CVI"/>
          <xs:enumeration value="PDF Test Chart"/>
          <xs:enumeration value="Other"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="Filename" type="nonNullString" use="required"/>
    <xs:attribute name="Comment" type="xs:string" use="optional"/>
  </xs:complexType>
</xs:element>
```

```xml
<Attachment
  AttachmentRef="ID000"
  DocType="PDF Test Chart"
  Filename="TestsRUs_1234.pdf"
  Comment="Some comment"
/>
```

### MiscAttribute

There isn’t much more to say about the `MiscAttribute` list that may
follow the attachments. These are here mainly for future use and
unanticipated needs. This element is simply a pair of `nonNullStrings`,
`Name` and `Value`. Their meaning is left as an exercise for the future.

```xml
<xs:element name="MiscAttribute">
  <xs:complexType>
    <xs:attribute name="Name" type="nonNullString" use="required"/>
    <xs:attribute name="Value" type="nonNullString" use="required"/>
  </xs:complexType>
</xs:element>
```

```xml
<MiscAttribute Name="SomeNewField" Value="42"/>
```

### Binary

`EquinePhotographs`, `BrandImages`, and `Attachments` are all binary “files”
with their content carried in the `Binary` elements at the end of the
`eCVI`. Each `Binary` element must carry three pieces of information.

First, the `Binary` must uniquely identify itself so it can be referenced
in one or more of the above elements. The attribute named `ID` is XML
datatype `xs:ID`. We’ve already encountered the `xs:NCName` datatype in the
`xs:IDREF` type. `xs:ID` is another `xs:NCName`. The difference is that `xs:ID`
must be unique across the whole document. It may—actually _must_ to make
any sense—match one or more `xs:IDREF` values elsewhere in the document.
This should be a simple identifier in a format that the eCVI application
can readily maintain uniqueness and easily reference. A common pattern
is “ID_123” where 123 is a sequence generated internally. Some may want
to distinguish between the above referencing elements such as “EP123”,
“BI_123” (for “equine photograph” and “brand image”), etc. but that
makes absolutely no difference to the XML parser.

Next, the receiving system needs to have some idea what the binary data
_are_. This means what they are at the computer processing level. Is
this a JPEG image or a PDF document or an Excel spreadsheet? What
_information_ is contained in them was carried in the specific
referencing elements. Media types are defined on the internet by
Multipurpose Internet Mail Extensions (MIME) type. These are defined by
the internet engineering task force RFC[^2] 6838 and maintained by the
internet assigned numbers authority (IANA). Whew!. These consist of two
parts. The basic type such as “text, “image,” or “application” etc. is
followed by a slash (/) and a subtype such as jpeg or pdf. For the
complete list see:
https://www.iana.org/assignments/media-types/media-types.xhtml. Or see a
more user-friendly list at:
https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics\_of\_HTTP/MIME\_types/Common\_types

The mime type of the `Binary` is carried in the required attribute
MimeType. The schema requires both basic type and subtype.

The rest of `Binary` is its one child element, Payload. Payload is an XML
datatype `xs:base64Binary`. Base 64 is a way of encoding the full range of
binary data in printable characters. It turns three bytes of binary into
four characters. This was invented for adding attachments to email
messages—as you can see in the name MIME.

So, the `Binary` `Payload` is essentially, one very long string of
nonsense-looking letters, digits and the + and / characters. It may have
= or == at the end. There are many slight variations on base64 encoding.
The one referred to by xs:base64Binary is defined by RFC 2045 that
describes MIME. Most of the variants are intended for use in URL
encoding, email headers, or other specific applications. See RFC 4648
for some details, if you care.

In general, the base64 encoders and decoders that come as common
programming language libraries work fine with the usual image and
application data files. There can be issues with base64 encoding
operating-system-specific Unicode text not embedded in an application
file structure such as PDF, or Word. Straight Unicode text shouldn’t
need to be sent in the Binary element because it can go in other
structured elements.

```xml
<xs:element name="Binary">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="Payload" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute name="ID" type="xs:ID" use="required"/>
    <xs:attribute name="MimeType" type="MimeType" use="optional"/>
  </xs:complexType>
</xs:element>
```

```xml
<Binary ID="ID000" MimeType="application/pdf">
  <Payload>
    JVBERi0xLjYKJeLjz9MKM
    . . . Pages and pages more . . .
    M4OQolJUVPRgo=
  </Payload>
</Binary>
```

# Regular Expression Patterns Used in the Schema Explained

Regular expressions are a very powerful tool for pattern matching in
text. They are widely used in power-user environments such as Microsoft
PowerShell, various Unix/Linux utilities such as grep and sed. Different
tools that use regular expressions have slightly different subsets of
patterns that are supported and interpretations of search parameters.
XML 1.0 supports a relatively restricted, but still very powerful, list
of regular expression features. When used as restrictions on string
variables, XML allows only strings that match against the provided
regular expression.

Because the regular expressions in XML patterns are used to validate
values applied to defined data fields, they must match the entire string
supplied. This is called being “implicitly anchored.” In most flavors of
regular expressions, to match on entire strings you must put a special
metacharacter at the beginning and end of the pattern. In most regular
expression settings, to find “this text” with nothing in front or in
back the pattern would be “^this text$”. XML regular expressions don’t
include the “^” or “$” anchors but still must match the whole string.
(The “^” still appears in its role in negation.) To allow “this text”
contained within anything else before or after in an XML pattern, you
would need, “.\*this text.\*” as your pattern. The “.\*” means “any
number of any characters.” In XML though, we almost always want to test
the full string against a pattern. So implicit anchoring makes sense.

The `xs:SimpleType` definitions allow a list of patterns such that values
that match any of the list are valid. The eCVI standard tries to
maintain something like readability while constraining valid values.
Sometimes splitting a complex pattern into a choice of two or three
makes the range of acceptable values easier to grasp.

In this chapter, I will briefly describe each of the data items that is
restricted by a regular expression pattern. A really useful feature of
good XML-specific editor software is often a tool for evaluating RegEx’s
that is specific to the XML subset of patterns.

#### Phone Number

```regex
\d{10}
```

The US phone number pattern is a simple “any ten digits.” The “\d”
means any digit and the “{10}” means “ten of these.” This could probably
be tightened up to include only starting with valid area codes but, for
now, is any ten digits.

#### InternationalPhone Number

```regex
(9[976]\d|8[987530]\d|6[987]\d|5[90]\d|42\d|3[875]\d|2[98654321]\d|9[8543210]|8[6421]|6[6543210]|5[87654321]|4[987654310]|3[9643210]|2[70]|7|1)\d{1,14}
```

This strange looking sequence expands to define all of the world’s one,
two and three digit telephone prefix codes followed by one to 14 more
digits. Most but not all of the prefixes are country specific. For
example the prefix we use in the US also covers US territories, Canada,
and several Caribbean island countries.

Inside the parentheses each segment separated by the | character is one
choice of sub-pattern. So for example, 9[976]\d means that “99”,
“97”, or “96” followed by any additional digit are all valid
telephone prefixes somewhere in the world. Further along we get to
9[8543210] that says that “98”, “95”, “94”, “93”, “92”, “91” and “90”
are all valid two-digit prefixes. Note the lack of a trailing \d in
that sub-pattern. Finally, after the parentheses comes \d{1,14} that
means that after the prefix may be one to 14 more digits. Because each
country establishes its own pattern for numbers, this RegEx cannot
validate every variation, but it will catch many invalid international
numbers. Note also that some implementations include the + character in
the phone number. Here it is not part of the data.

#### ZIP

```regex
\d{5}
```

```regex
\d{5}-\d{4}
```

The ZIP element in the USAddress may match either of two patterns for
the formatted five digit and nine digit zip codes.

#### nonNullString

```regex
.*[^\s].*
```

Where the standard needs to prevent entry of a totally blank string it
uses nonNullString. This pattern translates to any number of any
characters, followed by a character that is not white space, followed by
any number of any characters. This RegEx may look strange to those
familiar with similar “wild cards.” Here a period means “any character”
and the asterisk \* means “zero or more of these.” So, the only required
part is the \[^\\s\] which expands to “anything (the \[\]) except (the
^) any white space character (the \\s).”

#### EmailType

```regex
[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,24}
```

The familiar email address pattern translates to a name plus the @
symbol plus a domain. The name \[a-zA-Z0-9.\_%+-\]+ means any upper or
lower case letter, digit or characters: period, underscore, percent,
plus, or hyphen (minus) repeated one or more times. The + inside the
square brackets is literally a + while the one just past the bracket
means “one or more”. The domain, \[a-zA-Z0-9.-\]+\\.\[a-zA-Z\]{2,24} is
a little more constrained. One or more upper or lower case letters,
digits or the characters: period or hyphen followed by a literal period
followed by two to twenty four upper or lower case letters.

#### PremIdType

```regex
[A-Z0-9]{6,8}
```

The RegEx for the standard PIN or LID is very loose. It allows any six,
seven or eight character string with upper case letters and digits. This
allows USDA PINs with seven characters of state LIDs with six or eight
characters. Proper validation of PINs and LIDs requires use of the check
digit explained in Appendix A.

#### AgeType

```regex
(&lt;|&gt;)? ?\d{1,3}(\.\d+)? ?(d|wk|mo|a)
```

```regex
(19|20)\d\d-(0[1-9]|1[012])-(0[1-9]|[12]\d|3[01])
```

The AgeType can match either of two different RegEx patterns. The first
is one to three digits optionally preceded by a \< or \> symbol (as XML
entities) and optionally a space. It optionally includes a decimal
followed by one or more digits. That is followed by an optional space
and an age unit of d, wk, mo, or a; for day, week, month, or year.

The alternative is to provide a date of birth. This RegEx matches the
format of `xs:date`. YYYY-MM-DD. Year being constrained to this century
and last. So, look out for the Y2.1K bug to appear in the year 2100.

#### AINType

```regex
(840)\d{12}
```

The US Animal Identification Number (AIN) is defined by the US ISO
country code 840 followed by twelve additional digits

#### InternationalAINType

```regex
((004)|(008)|(010)|(012)|(016)|(020)|(024)|(028)|(031)|(032)|(036)|(040)|(044)|(048)
|(050)|(051)|(052)|(056)|(060)|(064)|(068)|(070)|(072)|(074)|(076)|(084)|(086)|(090)|(092)
|(096)|(100)|(104)|(108)|(112)|(116)|(120)|(124)|(132)|(136)|(140)|(144)|(148)|(152)|(156)
|(158)|(162)|(166)|(170)|(174)|(175)|(178)|(180)|(184)|(188)|(191)|(192)|(196)|(203)|(204)
|(208)|(212)|(214)|(218)|(222)|(226)|(231)|(232)|(233)|(234)|(238)|(239)|(242)|(246)|(248)
|(250)|(254)|(258)|(260)|(262)|(266)|(268)|(270)|(275)|(276)|(288)|(292)|(296)|(300)|(304)
|(308)|(312)|(316)|(320)|(324)|(328)|(332)|(334)|(336)|(340)|(344)|(348)|(352)|(356)|(360)
|(364)|(368)|(372)|(376)|(380)|(384)|(388)|(392)|(398)|(400)|(404)|(408)|(410)|(414)|(417)
|(418)|(422)|(426)|(428)|(430)|(434)|(438)|(440)|(442)|(446)|(450)|(454)|(458)|(462)|(466)
|(470)|(474)|(478)|(480)|(484)|(492)|(496)|(498)|(499)|(500)|(504)|(508)|(512)|(516)|(520)
|(524)|(528)|(531)|(533)|(534)|(540)|(548)|(554)|(558)|(562)|(566)|(570)|(574)|(578)|(580)
|(581)|(583)|(584)|(585)|(586)|(591)|(598)|(600)|(604)|(608)|(612)|(616)|(620)|(624)|(626)
|(630)|(634)|(638)|(642)|(643)|(646)|(652)|(659)|(660)|(662)|(663)|(666)|(670)|(674)|(678)
|(682)|(686)|(688)|(690)|(694)|(702)|(703)|(704)|(705)|(706)|(710)|(716)|(724)|(728)|(729)
|(732)|(740)|(744)|(748)|(752)|(756)|(760)|(762)|(764)|(768)|(772)|(776)|(780)|(784)|(788)
|(792)|(795)|(796)|(798)|(800)|(804)|(807)|(818)|(826)|(831)|(832)|(833)|(834)|(850)|(854)
|(858)|(860)|(862)|(876)|(882)|(887)|(894))\d{12}
```

The international AIN is really the same pattern as the US AIN but with
the twelve additional digits preceded by a choice of ISO country codes
other than US. Here the codes are simply listed rather than compressed
the way they were in the international phone codes. Note: this is a
different code list because these actually are country specific.

#### MfrRFIDType

```regex
((9[0-8]\d)|(9\d[0-8]))\d{12}
```

Manufacturer RFID tags start with three digits starting with 9 and
excluding 999. The RegEx reads 9 followed by 0 through 8 followed by any
digit, or 9 followed by any digit followed by 0 through 8. Those three
digits are followed by any additional twelve digits.

#### NUES9Type

```regex
(\d{2}|AL|AK|AZ|AR|CA|CO|CT|DE|DC|FL|GA|HI|ID|IL|IN|IA|KS|KY|LA|ME|MD|MA|MI|MN|MS|MO|MT|NE|NV|NH|NJ|NM|NY|NC|ND|OH|OK|OR|PA|PR|RI|SC|SD|TN|TX|UT|US|VA|WA|WV|WI|WY)[A-Z]{3}\d{4}
```

The National Uniform Eartag System (NUES) defines a nine character
pattern that would be fairly simple except that the first two
characters, which define the state of issue, may be either the state
postal code or the USDA numeric code for the state. Thus, any two digits
or any of this string of two-character codes. (The two digits could be
similarly constrained but aren’t.) The state portion is followed by
three upper case letters and four digits.

#### NUES8Type

```regex
\d{2}[A-Z]{2}\d{4}
```

The NUES8 pattern is easier because the state postal codes are not used.
It is any two digits followed by any two upper case letters and four
digits.

#### MimeType

```regex
.{1,127}/.{1,127}
```

The RegEx for MIME type simply shows that both the basic type and
subtype are required, separated by a / character. Both halves can be one
to 127 characters.

#### ISOCountryCode

```regex
((004)|(008)|(010)|(012)|(016)|(020)|(024)|(028)|(031)|(032)|(036)|(040)|(044)|(048)
|(050)|(051)|(052)|(056)|(060)|(064)|(068)|(070)|(072)|(074)|(076)|(084)|(086)|(090)|(092)
|(096)|(100)|(104)|(108)|(112)|(116)|(120)|(124)|(132)|(136)|(140)|(144)|(148)|(152)|(156)
|(158)|(162)|(166)|(170)|(174)|(175)|(178)|(180)|(184)|(188)|(191)|(192)|(196)|(203)|(204)
|(208)|(212)|(214)|(218)|(222)|(226)|(231)|(232)|(233)|(234)|(238)|(239)|(242)|(246)|(248)
|(250)|(254)|(258)|(260)|(262)|(266)|(268)|(270)|(275)|(276)|(288)|(292)|(296)|(300)|(304)
|(308)|(312)|(316)|(320)|(324)|(328)|(332)|(334)|(336)|(340)|(344)|(348)|(352)|(356)|(360)
|(364)|(368)|(372)|(376)|(380)|(384)|(388)|(392)|(398)|(400)|(404)|(408)|(410)|(414)|(417)
|(418)|(422)|(426)|(428)|(430)|(434)|(438)|(440)|(442)|(446)|(450)|(454)|(458)|(462)|(466)
|(470)|(474)|(478)|(480)|(484)|(492)|(496)|(498)|(499)|(500)|(504)|(508)|(512)|(516)|(520)
|(524)|(528)|(531)|(533)|(534)|(540)|(548)|(554)|(558)|(562)|(566)|(570)|(574)|(578)|(580)
|(581)|(583)|(584)|(585)|(586)|(591)|(598)|(600)|(604)|(608)|(612)|(616)|(620)|(624)|(626)
|(630)|(634)|(638)|(642)|(643)|(646)|(652)|(659)|(660)|(662)|(663)|(666)|(670)|(674)|(678)
|(682)|(686)|(688)|(690)|(694)|(702)|(703)|(704)|(705)|(706)|(710)|(716)|(724)|(728)|(729)
|(732)|(740)|(744)|(748)|(752)|(756)|(760)|(762)|(764)|(768)|(772)|(776)|(780)|(784)|(788)
|(792)|(795)|(796)|(798)|(800)|(804)|(807)|(818)|(826)|(831)|(832)|(833)|(834)|(850)|(854)
|(858)|(860)|(862)|(876)|(882)|(887)|(894))
```

Because the international AIN starts with the ISO country code, this
pattern is identical to that one except for not including the additional
12 digits.

# Enumerated Value Lists Used in the Schema

In the eCVI schema, lists of valid values are sometimes defined in-line
in the element or attribute that uses them but more often as a
stand-alone simple type definition. The choice depends on the length of
the list, the number of times it is referenced, and sometimes just what
looked right to the editor. They work the same in valid documents either
way.

Notice that almost all these lists are made up of human-readable text
rather than short numeric or alphanumeric codes. That is a tradeoff
between space efficiency and easy of reading for development, error
checking, etc. But these are technically codes in the sense that the
spelling, punctuation, and capitalization are all required to be exactly
as they are in the lists.

| **Code (Exact text required)** | **Description or Comments** |
| ------------------------------ | --------------------------- |

#### MovementPurposes

| Code                             | Description                                                                   |
| -------------------------------- | ----------------------------------------------------------------------------- |
| Racing                           |                                                                               |
| Sale                             |                                                                               |
| Grazing                          |                                                                               |
| Training                         |                                                                               |
| Slaughter                        |                                                                               |
| Medical Treatment                |                                                                               |
| Exhibition/Show/Rodeo            |                                                                               |
| Breeding                         |                                                                               |
| Competition                      | Competition other than racing, show, or rodeo covered by more specific codes. |
| Feeding to condition             |                                                                               |
| Feeding to slaughter             |                                                                               |
| Laying Hens                      |                                                                               |
| Hunting for harvest              |                                                                               |
| Companion Animal                 |                                                                               |
| Personal Travel/Transit          |                                                                               |
| Owner relocating                 |                                                                               |
| Evacuation from Natural Disaster |                                                                               |
| Other                            |                                                                               |

#### TransportMode

| Code  | Description                           |
| ----- | ------------------------------------- |
| air   |                                       |
| boat  |                                       |
| car   |                                       |
| rail  |                                       |
| truck |                                       |
| land  | Animals are driven on foot over land. |
| other |                                       |

#### DocType

| Code               | Description                                     |
| ------------------ | ----------------------------------------------- |
| Scanned Paper CVI  | Image of a paper CVI, often in PDF form.        |
| Scanned Test Chart | Image of a paper test chart, often in PDF form. |
| PDF CVI            | PDF rendering of an electronic CVI              |
| PDF Test Chart     | PDF rendering of an electronic test chart       |
| Other              |                                                 |

#### Type (Phone)

| Code      | Description |
| --------- | ----------- |
| Unknown   |             |
| Landline  |             |
| Cellphone |             |
| Fax       |             |

#### Status (BrucellosisiStateOrAreaStatus)

| Code               | Description |
| ------------------ | ----------- |
| Free               |             |
| Class A            |             |
| Class B            |             |
| Class C            |             |
| GYA, DSA (Class A) |             |

#### Status (TuberculosisStateOrZoneStatus)

| Code                                             | Description |
| ------------------------------------------------ | ----------- |
| Free                                             |             |
| Modified Accredited Advanced State or Zone (MAA) |             |
| Modified Accredited State or Zone (MA)           |             |
| Non Accredited State or Zone (NA)                |             |

#### ResultName

| Code    | Description                                                     |
| ------- | --------------------------------------------------------------- |
| RESULT  | The actual result of the test, often a numeric value or pos/neg |
| COMMENT | Interpretation of the test result or other commentary           |

#### TagType

TagType here is just for those identifier types for which there are no
specified format that can be validated against regular expression
patterns. Those are all named in defined element types above.

| Code    | Description                                                                                                                                |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| AMID    | American ID                                                                                                                                |
| BT      | Backtag                                                                                                                                    |
| IMP     | Implant (microchip). If the chip contains an official AIN, it should be in an AIN, or InternationalAIN element rather than OtherOfficialID |
| NAME    | Animal name when it has value as an identifier                                                                                             |
| SGFLID  | Scrapie group flock ID                                                                                                                     |
| NPIN    | Swine PIN Tag                                                                                                                              |
| PINPLUS | Swine PIN plus Management Tag                                                                                                              |
| TAT     | Tattoo                                                                                                                                     |
| OTHER   | Be very careful that the tag really is an official ID before using this type.                                                              |

#### SpeciesCodes

| Code | Description                             |
| ---- | --------------------------------------- |
| AQU  | Aquaculture                             |
| BEF  | Beef Cattle                             |
| BIS  | Bison                                   |
| CAM  | Camelid (Alpacas, Llamas, etc.)         |
| CAN  | Canine                                  |
| CAP  | Caprine (Goats)                         |
| CER  | Cervids                                 |
| CHI  | Chickens                                |
| DAI  | Dairy Cattle                            |
| EQU  | Equine (Horses, Mules, Donkeys, Burros) |
| FEL  | Feline                                  |
| OVI  | Ovine (Sheep)                           |
| POR  | Porcine (Swine)                         |
| TUR  | Turkeys                                 |

#### SexType

| Code               | Description |
| ------------------ | ----------- |
| Female             |             |
| Male               |             |
| Spayed Female      |             |
| Neutered Male      |             |
| True Hermaphrodite |             |
| Gender Unknown     |             |
| Other              |             |

#### GroupSexType

| Code               | Description |
| ------------------ | ----------- |
| Female             |             |
| Male               |             |
| Spayed Female      |             |
| Neutered Male      |             |
| True Hermaphrodite |             |
| Mixed Group        |             |
| Gender Unknown     |             |
| Other              |             |

#### CommodityType

| Code                          | Description |
| ----------------------------- | ----------- |
| Embryos                       |             |
| Hatching Eggs                 |             |
| Liquid Egg (Non-Pasteurized)  |             |
| Liquid Egg (Pasteurized)      |             |
| Milk (Pasteurized)            |             |
| Milk (Raw)                    |             |
| Mohair/Cashmere               |             |
| Shell Eggs (Nest Run)         |             |
| Shell Eggs (Washed/Sanitized) |             |
| Shells/Inedible Egg Product   |             |
| Semen                         |             |
| Wool                          |             |

#### DiseaseType

| Code                               | Description |
| ---------------------------------- | ----------- |
| Avian Influenza                    |             |
| Bovine Viral Diarrhea Virus        |             |
| Brucella abortus                   |             |
| Brucella canis                     |             |
| Brucella ovis                      |             |
| Brucella suis                      |             |
| Caprine Arthritis and Encephalitis |             |
| Corynebacterium pseudotuberculosis |             |
| Equine Infectious Anemia           |             |
| Pseudorabies                       |             |
| Rabies                             |             |
| Salmonella pullorum                |             |
| Tritrichomonas foetus              |             |
| Tuberculosis                       |             |

#### StateCodeType

```
| AA | AE | AK | AL |
| AP | AR | AS | AZ |
| CA | CO | CT | DC |
| DE | FL | FM | GA |
| GU | HI | IA | ID |
| IL | IN | KS | KY |
| LA | MA | MD | ME |
| MH | MI | MN | MO |
| MP | MS | MT | NC |
| ND | NE | NH | NJ |
| NM | NV | NY | OH |
| OK | OR | PA | PR |
| PW | RI | SC | SD |
| TN | TX | UT | VA |
| VI | VT | WA | WI |
| WV | WY |
```

To save space most postal codes are left self-explanatory. The others
are territories such as GU Guam, MP Northern Mariana Islands, MH
Marshall Islands, etc. For details see:
https://faq.usps.com/s/article/What-are-the-USPS-abbreviations-for-U-S-states-and-territories.

#### PhotoView

| Code  | Description |
| ----- | ----------- |
| Left  |             |
| Front |             |
| Right |             |

#### ApprovalListType

These are used only in the generic Movement root element to support
movement on NPIP forms. They are not found in the original eCVI.

| Code               | Description                                                                                                                                            |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| NPIPParticipation  | Flocks or owners participate in NPIP and receive an NPIP-specific number                                                                               |
| NPIPClassification | Flocks can be classified as clean or monitored for various diseases based on frequency of testing. See NPIP documentation for list of classifications. |

# Transmission and Other NASAHO Requirements

For an eCVI application to “comply with the AAVLD/USAHA eCVI data
standard just means that the data files it generates are well-formed XML
that is valid with respect to the standard schema. As you have seen in
the earlier part of this guide, much effort has gone into designing the
schema to allow all reasonable combinations of values while rejecting
(invalidating) those that are clearly errors. But there is much more to
making an eCVI an acceptable substitute for government-issued paper
forms. Rather than require eCVI developers to contact each state animal
health authority individually to receive application requirements and
gain approval, the National Assembly of State Animal Health Officials
(NASAHO) established the eCVI Standards Subcommittee under the
Traceability and Technology Committee to review applications and
recommend approval of those that satisfy a common set of requirements.
The ability to produce data files that comply with the eCVI data
standard is an important one of those requirements, but there are many
more. This chapter will summarize those requirements.

## Additional Data Element Requirements

Some data elements that are optional in the standard schema are made
mandatory by the NASAHO committee. Plus, many of the conditionally
required elements that cannot be enforced by XML schema are stated
requirements of the committee and are tested by the committee during the
evaluation process.

Origin, destination, consignor, consignee, and all the variations on
animal and group identification must all comply with regulatory
requirements and be populated in the appropriate XML locations.

One data requirement of the NASAHO committee does not have a specific
field in the standard XML. The total number of animals covered by the
ICVI must be included. This is not included in any one data item in the
standard because it should, if all elements are correctly implemented,
but computable from individual `Animal`, and `GroupLot` elements.

Both inspection date and issue date are required by the committee. This
would, presumably, make the `InspectionDate` attribute of `GroupLot`
required even though it is optional in the schema. Breed, sex, and age
are also required by the committee but optional in the schema. These
differences may be to allow for the very rare exceptions that the
committee process can account for but a deterministic test like schema
validation cannot.

The NASAHO committee requires the national accreditation number be
provided. This is optional in the schema. In this case the explanation
is an intentional effort to move in the direction of NVAP validation as
part of the CVI process. One could anticipate the accreditation number
becoming required in a future version of the standard schema.

## Electronic Signature

The eCVI must be electronically signed by the veterinarian. This gets
more complicated that it first seems. The Electronic Signatures in
Global and National Commerce Act (Public Law 106-229) (E-SIGN), allows
for various different technical implementations of electronic signature
including, but not limited to, cryptographic digital signatures. The
committee adds some requirements around those established by E-SIGN. If
public key digital signatures are used, the provider must explain how
animal health officials can access the public keys to validate the
signature. If some other form of electronic signature is used, the
provider must be able to demonstrate the ability to detect changes and
ensure authenticity of the signature.

A useful way to think about electronic signatures is to imagine a
scenario in which the signature is challenged. For example, imagine that
a valuable animal dies in transit from a condition that a reasonable
veterinarian would have detected on examination. Now imagine that the
signature on the eCVI was forged because no exam had been made, or that
the veterinarian had signed the eCVI but now used as a defense that the
eCVI was forged or changed. Can you effectively demonstrate to a judge
that your signature implementation makes these claims impossible or at
least extremely improbable?

See Appendix B for more information on the legalities of electronic
signatures than you could ever want to know.

## Rendering of the Data as PDF and XML

ICVIs serve multiple purposes. For some of these, the information they
contain must be readily accessible to human readers such as livestock
inspectors, airline check-in agents, show officials, etc. For this
purpose, the NASAHO committee requires the eCVI application to be able
to generate a PDF document image of all the required information. But
the information in ICVIs has become much more important as data for
electronic processing, storage, retrieval, and analysis. For this
application, the information must be available as an XML “file” that
validates against the eCVI standard schema. When reviewing applications,
the committee takes great pains to ensure that everything the
veterinarian enters is rendered consistently on both the PDF and in the
appropriate structured location in the XML. A very common error is to
find key structured data found in generic locations such as Statements
or Description. Those locations are for specific uses, not to hold
individual structured data.

## Delivery

The provider of an eCVI application must be able to deliver both the PDF
and XML data to state animal health officials in both the sending and
receiving states. The method of delivery is left general but includes
email and API access as requested by the respective officials. This must
not require subscription to any specific network service.

If the provider includes the capability to void and/or revise eCVIs,
they must ensure that the void and revisions are delivered to both
origin and destination state animal health officials as well.

## License and Accreditation

The eCVI provider must limit issuing of eCVIs to veterinarians licensed
and accredited in the origin state. Many applications have provisions
for some data entry to be performed by unlicensed staff, however final
signature and issuance of the eCVI must be by the accredited
veterinarian. The provider must also notify the animal health officials
in the state of origin when a new veterinarian is added. This gives the
issuing state a chance to confirm license and accreditation status.

## Information Technology Support

Providers applying for approval must agree to provide technical support
for their product. They must demonstrate the technical ability to
validate the generated XML “files” against the standard eCVI XML schema.

## Flexibility

The fundamental goal of the NASAHO and its eCVI Standards Committee is
to see increased adoption of quality electronic CVIs. To that end, the
committee will consider reasonable alternative means of accomplishing
any of its requirements. The exception is validation against the eCVI
schema that is absolutely required.

Each state and territory veterinarian retains the right to approve or
disapprove any eCVI application for use in their state. But they all
understand the need for consistency and will override the recommendation
of their eCVI Approval Committee only in very special situations.

# Supported Variations

In this chapter, I will look at the other root elements that reuse much
of the structure and content of the eCVI element. Rather than repeat
everything, I will focus specifically on the differences from the eCVI.

## Movement

The `Movement` document type supports documentation of animal and animal
product movements that fall under regulatory oversight, but that do not
require the participation of a named veterinarian. This was motivated by
two main use-cases. The USDA animal disease traceability rules allow
some interstate movement on what are known as “alternative movement
documents.” These must be approved by the state animal health officials
in the sending and receiving states. They are often used to support
movement from stockyards to farms in neighboring states. The second
use-case is movement of poultry and hatching eggs under the National
Poultry Improvement Plan (NPIP). The NPIP allows shipments from approved
facilities to take place without individual veterinary inspection using
forms approved by the NPIP and based on participation in an NPIP
program.

Other than the absence of a `Veterinarian` element, the `Movement` element
is very nearly identical to the `eCVI` element. The only minor changes to
reflect the nature of “alternative movement documents.” The attributes
`CviNumber` and `CviNumberIssuedBy` are simply renamed to `MovementId` and
MovementI`dIssuedBy.

### NPIP Movements

One of the important use-cases for the generic movement document is
support for electronic NPIP 9-3 movements. A few details are specific to
this use. The `MovementId` is generated algorithmically from the source
flock’s NPIP participant number. These are issued serially for each
flock. The movement Id ends up being SS-FFF-123 where SS is the state
postal code, FFF is the state issued unique participant identifier and
123 is a number that increases by one for each shipment. Because the
number is based on an NPIP algorithm, the `MovementIdIssuedBy` should be
“NPIP.”

The key addition in `Movement` is a new element named `Approval` based on
the type `ApprovalType`. This consists of a `Type` and `Date` of the approval
as required attributes and a Person element to hold one or more
regulatory authorities that approved the movement. There are currently
only two defined approval types: `NPIPParticipation` and
`NPIPClassification`. These differ based on the various National Poultry
Improvement Plan programs that allow movement of birds and eggs on NPIP
form 9-3 instead of an ICVI.

## Sighting

It is often useful to collect much of the same information that goes
into a CVI but for animals that are in one location. This may be used
later to populate eCVIs or for management or other information
processing purposes. Having these data in a format that is compatible
with the eCVI will, hopefully, improve data interoperability and
management efficiency.

`Sighting` is even simpler than `eCVI` or `Movement`. A simple `xs:string`
attribute identifies the `SightingType`. The `SightingDate` attribute is
optional as is the `SourceSystem` that is similar to the
`CviNumberIssuedBy`. There is only a single `Location` `PremType` element
rather than separate `Origin` and `Destination`. And `Sighting` only applies
to one or more individual Animals.

# Suggestions on Software Development Practices

When a veterinarian issues an interstate certificate of veterinary
inspection (ICVI), they generally think of their job as conducting the
inspection, while creating the document is just “paperwork.” Perhaps the
veterinarian thinks of understanding the requirements for movement of
those specific animals between the two states as an important part of
the job, but that is likely not high on their list either. So, the key
purpose of an electronic certificate of veterinary inspection is not
just to eliminate the “paper” part of the paperwork. The real purpose is
to make it fast and easy to produce an ICVI that meets the requirements
of the seller, buyer, shipper, airline in some cases, and the animal
health authorities in both states and at USDA.

Unfortunately, development of software with requirements like these is a
dying art. Today most apps need to keep their users engaged for as
_long_ as possible to generate clicks for advertisers and data for AI.

Neither the eCVI data standards workgroup nor the NASAHO eCVI standards
committee wants to try to tell developers how to design their software.
That is best left to the skill and creativity of the various companies,
teams, and individual developers doing the work. But there have been
clear ideas about best practices that have come out over the decade or
so of experience with the standards and the software that supports them.
This chapter will look at some of the suggestions that have come from
both committees as they have attempted to deal with frequently seen
errors and other problems with ICVIs.

### XML Development Process

Many readers of this guide will be much more accomplished programmers
than me. They should feel free to skip this section or read-on and send
suggestions and corrections. This guide took a breadth-first approach,
describing the high-level structure first before digging down to the
details. For development, a bottom-up approach may be more efficient.

Mastering and assembling the best XML tools for the preferred
development environment should be a/the first step. Trying to add this
functionality after-the-fact can be very frustrating. Next, the eCVI XML
includes many defined simple data types. Building each of these as a
class, widget, or other modular component greatly simplifies
construction of the larger structure later. It can tie in nicely with
the kinds of error-checking we discuss in this chapter. Designing these
components to facilitate maintenance of patterns and value lists will
save time in the future.

Test handling of extended characters and XML entities early. Just about
every programming environment that supports XML has quirks in how it
handles extended characters such as diacritical marks for accented
letters. For ordinary characters that require XML entities it is
important to learn early in the process when you need to account for
them and when the software is doing so. XML encoding software such as
programming languages often do the entity encoding and decoding for you.
Good, so what is the issue? If you aren’t on the same page with your
software you can end up with goofy things like `&amp;lt;` or worse. For
an example in pseudocode: `if(mystring equals xmlelement.getText()…)`
would the entity have been resolved in `getText()` or does the calling
method need to account for that? Be sure to check your entity characters
early in your development process and at each stage of encoding and
decoding your data.

The tree structure of the eCVI lends itself to a design in which each
element defines itself and adds child elements and attributes to itself,
and those elements then define themselves inside the call that adds
them. Once the basic data types have been implemented, the tree can be
built either bottom up or top down.

It pays to start validating against the standard eCVI XML schema early
in the development process even if this means ignoring errors in
incomplete parts of the document or filling those areas with dummy valid
XML. (Some validating XML editors are better than others at showing all
the errors in a way that lets you ignore some. Others may stop at the
first invalid item.)

### Correct Data Starts with the User Interface

While validation against the standard XML schema is an important quality
check, it comes too late in the process to be user-friendly. Only at the
point of data-entry does the software have the opportunity to help the
user do it right the first time rather than just catching their mistake
later. The standard schema has many features designed to help with this.
Take the `AnimalTags` element for one example. The regular expressions
that define the tag types can be used to provide immediate feedback on
things like AINs that are too long or too short. This is a _very_ common
error that is very easy to check and correct if done at the point the
value is entered. Was it 840000123456789 or 8400000123456789? Your eye
gets lost in the string of zeros even in print. How will a busy
veterinarian do on a phone or tablet?

### Conditionality

XML schema language 1.0 does not allow for validation of one field based
on the value in another. Software, on the other hand, has access to
everything entered up to the current field. Incorporating an
understanding of conditionality into the workflow will give the software
a chance to check the value being entered against requirements for that
specific movement. For example, if the species has already been entered
and the CVI is for a horse, the `Accession` and `Test` for the Coggins test
can appear as required. Or if the veterinarian tries to enter a `GroupLot`
of one year old heifers, the software could remind them that they will
need official identification for each animal. Nothing says that the
program cannot help the veterinarian enter all the common data once and
expand into individual Animal records internally. These are just a
couple of hypothetical examples of how an eCVI app might use
conditionality to speed and simplify data-entry.

### “Almost Required” Data Items

Many data items are listed as “optional” in the XML schema—or allow “Not
Provided” as a placeholder. These are often only because of very rare
cases when the data do not exist for legitimate reasons. It should not
be impossible to create the eCVI in these cases but a good eCVI
application will make it clear that a value is expected. Doing so is a
“best practice.” _How_ to do so is best left to individual software
developers.

### Abuse of the “Other” Value

A major topic of discussion at both standards committees is what has
come to be called, “other abuse.” As I have tried to point out
throughout this guide, many enumerated value lists include “other” as a
choice, with a place to add free text for the value. These are in places
where it is impossible to anticipate every rare value. The zoo
veterinarian writing a CVI for an aardvark or zebra wouldn’t expect to
find these in the list of standard species codes\! It is far too common,
however, to find “other” used to enter something like “cattle” because
the UI didn’t make it easier to select an enumerated value than to just
type a value in “other.” A suggested rule of thumb is that in any list
with “other” as a choice, no more than 10% of values should be “other.”
In the context of eCVIs I think 5% or less should be the absolute
maximum. An eCVI developer might monitor the frequency of “other” in
various fields to find places for improvement in its UI. State animal
health officials will be watching for other abuse. Even if their wrath
falls on the veterinarians rather than the eCVI developer, the customer
will eventually be unhappy.

### Regulatory Compliance

At various places in this guide, I have referred to the code of federal
regulations and various state laws and regulations. While it is
ultimately up to the veterinarian to ensure that all regulatory
requirements have been met, there is great opportunity for the eCVI
application to add value by using the data that have been entered to
help with this. Think of tax software that goes through features of the
tax law to look for deductions the user may have missed. The
opportunities in this area are endless. This could be simply comparing
inspection date to issue date to be sure they are in the legal range. Or
it could include a complex rule-base drawn from analysis of the CFR, the
data in InterstateLivestock.com and other sources. The details are,
again, left to individual software providers.

### Finally, On the Way Out . . . (Validation)

And, finally, before the eCVI gets delivered—perhaps just before it is
signed—the associated XML data file should be validated against the
standard schema. It is a safe bet that the data files _will_ be
validated by the receiving system of the sending and/or receiving state
animal health officials. If the data file fails simple XML schema
validation, the authorities are not going to have much confidence in the
rest of the application. These issues are often referred to the National
Assembly committee and are reviewed periodically.

There was a time when the rule of thumb was to validate during
development and testing but turn off validation for production to
improve speed. As validation software, the platforms it runs on, and the
computers those run on have all improved over the years, that is no
longer reasonable. Validation of a document the size of an eCVI takes
milliseconds compared to the many minutes of data-entry involved.

# Appendices

## A: Premises Identification Number Check Digit Validation Algorithm

The regular expression that defines the Premises Identifier in the eCVI
standard schema can only check that it is six to eight alphanumeric
characters. To guard against data entry errors that are very easy to
make, the PIN and LID specification includes a check digit, the last
character of the ID, based on the ISO standard 7064, specifically the
Mod 37, 36 algorithm. This appendix is taken with very minor
modifications—to shorten the example to seven digits—from Appendix E of
version 4.1 (2004) of the US Animal Identification Plan (USAIP)
documentation.

An interesting fluke of the Mod 37, 36 algorithm is that while it
catches all single character errors, it can be fooled by some
two-character errors. And one of those involves zeros and O’s. Two zeros
entered as O’s will validate. And, wouldn’t you know, USDA began issuing
PINs with both zeros and O’s and with the first million or so starting
with two zeros. They stopped using O’s before they got to any PINs
starting with an O in either of the first two places. So, a good
additional check is for leading O’s in place of zeros. “Oh my\!”

### ISO 7064, Mod 37, 36

The check digit algorithm referenced has been taken from ISO 7064:1983,
Data Processing – Check Character Systems. It maps a string of
alphanumeric characters to a single alphanumeric character.

### Formula for Calculating the Check Digit for a 7 Character Identifier

The first six digits of our PIN are “A12425”. The characters of the
Identifier are processed character by character from left to right. N=7
is defined as the number of characters including the check digit in the
identifier. The characters of the Identifier (including the check digit)
are numbered from right to left: a<sub>1</sub> is the check digit and
a<sub>2</sub> to a<sub>7</sub> are the characters of the Identifier as
follows. Please see Figure 1.

| A             | 1             | 2             | 4             | 2             | 5             | x             |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| a<sub>7</sub> | a<sub>6</sub> | a<sub>5</sub> | a<sub>4</sub> | a<sub>3</sub> | a<sub>2</sub> | a<sub>1</sub> |

The algorithm then comprises five steps:

Step 1: Set a<sub>j</sub> for j=n...2 as follows:

> a<sub>n</sub> is the value for the first character of the identifier
> (see Table 1);  
> a<sub>n-1</sub> is the value for the second character of the
> identifier;  
> ...
Animals have not been exposed to kryptonite
Step 2: Set j=1 and P1=36

Step 3: Calculate

> S<sub>j</sub> = Pj|37 + a<sub>(n-j+1)</sub>
>
> P<sub>(j+1)</sub> = S<sub>j||36</sub> x 2
>
> For j=1...n, where  
> ||<sub>36</sub> is the remainder after division by 36. If the
> remainder equals zero, then ||36 = 36.  
> |<sub>37</sub> is the remainder after division by 37 (never equals to
> 0).  
> a<sub>(n-j+1)</sub> is value of a character in the string.

Step 4: The check digit a1 must be computed so that Sn<sub>||36</sub> =1.

Step 5: Use Table 1 to select the Check Character.

| Table 1 |       |      |       |      |       |
| ------- | ----- | ---- | ----- | ---- | ----- |
| Char    | Value | Char | Value | Char | Value |
| 0       | 0     | A    | 10    | N    | 23    |
| 1       | 1     | B    | 11    | O    | 24    |
| 2       | 2     | C    | 12    | P    | 25    |
| 3       | 3     | D    | 13    | Q    | 26    |
| 4       | 4     | E    | 14    | R    | 27    |
| 5       | 5     | F    | 15    | S    | 28    |
| 6       | 6     | G    | 16    | T    | 29    |
| 7       | 7     | H    | 17    | U    | 30    |
| 8       | 8     | I    | 18    | V    | 31    |
| 9       | 9     | J    | 19    | W    | 32    |
|         |       | K    | 20    | X    | 33    |
|         |       | L    | 21    | Y    | 34    |
|         |       | M    | 22    | Z    | 35    |

Example

| j   | Char | Char Value | A<sub>(n-j+1)</sub> | P<sub>j</sub> | P<sub>j | 37</sub> | S<sub>j</sub> | S<sub>j |     | 36</sub> | P<sub>j+1</sub> |
| --- | ---- | ---------- | ------------------- | ------------- | ------- | -------- | ------------- | ------- | --- | -------- | --------------- |
| 1   | A    | 10         | 10                  | 36            | 36      | 46       | 10            | 20      |
| 2   | 1    | 1          | 1                   | 20            | 20      | 21       | 21            | 42      |
| 3   | 2    | 2          | 2                   | 42            | 5       | 7        | 7             | 14      |
| 4   | 4    | 4          | 3                   | 14            | 14      | 18       | 18            | 36      |
| 5   | 2    | 2          | 2                   | 36            | 36      | 38       | 2             | 4       |
| 6   | 5    | 5          | 5                   | 4             | 4       | 9        | 9             | 18      |
| 7   |      |            |                     | 18            | 18      |          |               |         |

S<sub>7</sub> is defined as S<sub>7</sub> =
P<sub>7|37</sub>+a<sub>1</sub> (a<sub>1</sub> being the Check
Character). Hence, we must find an a<sub>1</sub>, so that 18+
a<sub>1</sub>-1 is dividable by 36 without rest.

This leads to a1= 19, which represents the character “J”. Hence the
complete “Identifier” is A1- 2425J.

### Over Simplified Java Language Implementation

A production version of this implementation would include additional
error handling, etc. This version simply checks a PIN for a valid check
digit. This uses nothing very Java-specific, the code is almost
identical in C\# and similar in Python, etc.

```java
public class PremIDCheckSum {
  private static char[] char36 = {
      '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
      'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J',
      'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T',
      'U', 'V', 'W', 'X', 'Y', 'Z', '*'
  };

  /**
   * This class is not instantiable. Only used as static method container.
   */
  private PremIDCheckSum() {
  }

  /**
   * Calculate a checksum character based upon the identifier less checksum
   *
   * @param sID String Identifier without checksum
   * @throws Exception If the identifier contains characters other than
   *                   digits or capital letters.
   * @return char Checksum
   */
  public static char getChecksum(String sID) throws Exception {
    int pj = 36;
    int sj = 0;

    for (int i = 0; i < sID.length(); i++) {
      char cNext = sID.charAt(i);
      int iNext = lookup(cNext);
      if (iNext == -1) {
        throw new Exception("Character " + cNext + " is not valid in ID");
      }
      sj = pj + iNext;
      sj = sj % 36;
      if (sj == 0) {
        sj = 36;
      }
      pj = (sj * 2) % 37;
    }

    sj = (37 - pj) % 36;
    if (sj < 0 || sj >= char36.length) {
      throw new Exception("Invalid numerical result: " + sj);
    }
    return char36[sj];
  }

  /**
   * Check the identifier with checksum for validity.
   *
   * @param sID String Identifier with checksum
   * @throws Exception If the identifier contains characters other than
   *                   digits or capital letters.
   * @return boolean true if last character is correct checksum.
   */
  public static boolean isValid(String sID) throws Exception {
    char cCheckSum2 = sID.charAt(sID.length() - 1);
    String sID2 = sID.substring(0, sID.length() - 1);
    char cCheckSum = getChecksum(sID2);
    return cCheckSum == cCheckSum2;
  }

  private static int lookup(char cIn) {
    for (int i = 0; i < char36.length; i++) {
      if (char36[i] == cIn) {
        return i;
      }
    }
    return -1;
  }

  public static void main(String[] args) {
    try {
      System.out.println(args[0] + (isValid(args[0]) ? " is valid" : " is not valid"));
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}
```

## B: Electronic Signature Considerations

I originally wrote this whitepaper for the Connecticut Hospital
Association in 2003 as hospitals were coming to grips with the
requirements for digital interoperability under the health information
portability and accountability act (HIPAA). Most people now think of
HIPAA as just the security part, but that was to facilitate the
electronic movement of health information. Secure electronic signatures
were a key part of that. Electronic signatures had only recently become
law of the land under the E-SIGN legislation. That original whitepaper
has been edited for this appendix.

In terms of purpose and effect, it is important to understand that an
“electronic signature” is really no different than a “signature” as it
is ordinarily understood. What is the purpose of a signature? The
American Bar Association describes it this way:

“A signature is not part of the substance of a transaction, but rather
of its representation or form. Signing writings serve the following
general purposes:

1.  “**Evidence:** A signature authenticates a writing by identifying
    the signer with the signed document. When the signer makes a mark in
    a distinctive manner, the writing becomes attributable to the
    signer.

2.  “**Ceremony:** The act of signing a document calls to the signer's
    attention the legal significance of the signer's act, and thereby
    helps prevent inconsiderate engagements

3.  “**Approval:** In certain contexts defined by law or custom, a
    signature expresses the signer's approval or authorization of the
    writing, or the signer's intention that it have legal effect.

4.  “**Efficiency and logistics:** A signature on a written document
    often imparts a sense of clarity and finality to the transaction and
    may lessen the subsequent need to inquire beyond the face of a
    document. Negotiable instruments, for example, rely upon formal
    requirements, including a signature, for their ability to change
    hands with ease, rapidity, and minimal interruption."[^3]

Pen and ink signatures have generally been accepted for these purposes
largely due to the difficulty involved in creating a “copy” of a
signature or of altering the document after it has been signed without
the alteration being apparent. These traditional signatures depend upon
the biometric aspects of an individual's handwriting as well as chemical
properties of ink and paper to make technical assessment of originality
reasonably trustworthy.[^4] In the case of an individual who cannot
write his or her name, an X is often used to sign documents. In this
case handwriting analysis cannot be used to authenticate the signer.
Instead, the signature is witnessed to verify that the X was written by
the named individual with the intent to sign. In general, the signature
is a device applied to the paper upon which a document is recorded. It
is placed there by an individual who is identifiable from the signature
and who has exercised some degree of care in their intent to sign the
document. An electronic signature should be essentially the same thing
for an electronic document.

The Electronic Signatures in Global and National Commerce Act (E-SIGN)
defines an electronic signature: “The term 'electronic signature' means
an electronic sound, symbol, or process, attached to or logically
associated with a contract or other record and executed or adopted by a
person with the intent to sign the record."[^5] This definition leaves
the details of how the signature should be “attached or logically
associated” completely open for the parties to the contract or record to
define. Just as a pen and ink signature is associated with a document by
being placed on the same sheet of paper, usually after the text of the
document, some mechanism must exist to bind the electronic signature to
the document that it authenticates.

The United States Department of Justice (DOJ) has provided some guidance
on the requirements for implementation details in its advice to federal
agencies needing to implement electronic signatures. “GPEA \[Government
Paperwork Elimination Act, 44 USC 3504\] Section 1707 provides that
certain electronic records or signatures ‘shall not be denied legal
effect, validity, or enforceability because such records are in
electronic form.’ (Section 101 of E-SIGN contains similar language about
the validity of electronically recorded commercial transactions.) While
that wording bars courts from invalidating electronic records and
signatures merely because they are in electronic form, it does not
require courts to accept electronic records and signatures that are
deficient in other respects. For example, if there are reasons to doubt
that it was actually the electronic signature holder who affixed the
signature in question, a court might not accept the electronic
signature, just as it might decline to accept a paper signature that
could not be verified.”[^6]

The DOJ goes on to list attributes of an electronic signature. This list
or variations of the same have been widely quoted throughout the
electronic security community by both legal and technical experts. “The
ideal electronic signature system deployed by an agency would produce
electronic signatures that are:

1.  “Unique to the signer

2.  “Under the signer's sole control

3.  “Capable of being verified by a third party

4.  “Linked to data in such a manner that changes to the data invalidate
    the signature

"The degree to which these attributes are necessary depends on the risks
of the particular transaction.” [^7]

### Electronic Signature Requirements

These requirements begin to take on specific characteristics that can be
mapped to technical requirements.

#### Identification:

The first requirement addresses the technical issue of “user
identification.” The signature must be unique to a single individual.
This seemingly simple requirement is, in fact, a significant challenge.
Global identifiers are a challenge at best. The best we can usually
accomplish is to ensure that an individual is uniquely identified within
a specific domain such as an organization, government, or corporation.
The identification and domain need to be recorded as part of the
signature.

#### Authentication:

Often confused with identification is the issue of “user
authentication.” The DOJ addresses this by requiring the electronic
signature to be “under the signer’s sole control.” Not only does the
signature need to uniquely identify the signer, it must prove that the
signer, and no one else, applied the signature. A rubber signature
stamp, as typically used, is an example of a technique that complies
with the first requirement while failing the second. A good electronic
signature implementation must use a strong method of authenticating the
signer. It is common practice to describe the strength of authentication
in terms of three factors: something you know (PINs or passwords),
something you have (keys, smartcards or tokens), and something you are
(biometrics). For the most part any single factor is relatively weak.
Strong authentication is generally considered to require two or three
factors. A smartcard plus a Protected Identification Number (PIN) is
two-factor authentication, “something you have” plus “something you
know.” A PIN plus a password is still one-factor authentication because
each is “something you know.”

#### Independent Verifiability:

The third item requires verifiability by a third party addresses the
issue of portability. This is obviously essential for signed documents
intended to be transmitted to a second party but is also important for
long term storage of documents in which the signature may need to be
verified long after the system that created it has been retired. This is
likely to present challenges for any electronic signature system, but
most experts agree that systems based on widely adopted standards are
likely to have better portability than those based on proprietary
designs.

#### Data Integrity:

The requirement for the signature to be “linked to data” to make changes
apparent addresses the technical issue of “data integrity.” This
requirement is sometimes stated as “data authentication.” This is a
distinct issue from user authentication. An advantage of electronic data
processing is the ease with which edits can be applied to the contents
of a digital document. After a document has been signed, this strength
becomes a liability. Whereas changes to a signed paper document are
supposedly apparent—sometimes a dubious supposition—electronic edits are
indistinguishable from original data unless special features are in
place to ensure data integrity. In the ideal electronic signature,
document integrity is an inherent feature of the signature itself. In
other systems document integrity is left to other system functionality.

A subtle but significant issue arises around the data integrity
requirement. In cryptography literature creating a digital signature is
described as “computing a signature over” a block of data. Selecting the
data to be included in the computation of the signature is critical.
Take for example a data entry form. Should the signature be computed
over just the answers, or must it include the contents of the form as
well? How meaningful are the answers taken out of context? An analogy
from the pen and ink signature domain is the signature on the last page
of a multi-page document. In this case there is minimal protection
against substitution of false earlier pages. The electronic counterpart
allows the system to apply data integrity control to exactly those data
the signer is authenticating. The user interface must make this very
clear to the signers so that they are aware of precisely what they are
signing.

#### Intent to Sign:

The user interface issue leads to another consideration. Not listed but
implied in the DOJ’s requirements above is the indication of “intent to
sign.” The same technologies commonly used in electronic signature are
often used for simple source and document authentication. This type of
application would not meet the legal definition of a signature.
Indicating clear intent may require additional features equivalent to
the signature block and date used to indicate intent to sign in a pen
and ink signature. Some systems require the signer to enter their
password or PIN for each formal signature. At a minimum a good interface
will require clicking a clearly labeled “sign now” button. Such is not
the case with many currently available commercial applications.

Even where the computer makes clear the content and intent to sign, the
user must trust the computer. Bruce Schneier described the trusted
computer issues this way. “Digital signatures prove, mathematically,
that a secret value known as the private key was present in a computer
at the time Alice's signature was calculated. It is a small step from
that to assume that Alice entered that key into the computer at the time
of signing. But it is a much larger step to assume that Alice intended a
particular document to be signed. And without a tamperproof computer
trusted by Alice, you can expect ‘digital signature experts’ to show up
in court contesting a lot of digital signatures.”[^8]

#### Risk Assessment:

The last issue addressed by the DOJ’s list of features is the note at
the end that links these requirements to a risk assessment. If one
technology existed to suit all electronic signature needs, this would
have been specified in E-Sign and other regulations. In fact, there are
a wide range of electronic signature technologies available, and each is
appropriate in some applications and not in others. The process of
selecting a technology must begin with an analysis of the risks and
benefits provided by the electronic process.

It is helpful to view risks in two categories. First, there is the risk
of actual compromise. This type of risk is what we deal with in every
electronic process we implement. Controls must be in place to prevent
loss, alteration or fabrication of data by random failures, intentional
alteration or deletion, or malicious intrusion. The consequences of such
loss or alteration may vary from simple inconvenience in the case of a
lost work in progress, to significant financial loss in the case of lost
vital business records, to potential serious injury or loss of life in
the case of altered clinical documents. The analysis of this type of
risk, as well as the potential controls, are firmly in the technical and
user domains.

A second type of risk involves more complicated legal analysis. The term
“non-repudiation” is used, in the context of electronic signatures, to
indicate that the signer will not be able to deny the act of signing.
Repudiation does not require any actual compromise of the system. Simply
demonstrating the potential for such a compromise to have occurred may
be sufficient to allow a signer to escape responsibility for a signed
document. A closely related risk that does not require actual compromise
is the risk that a regulatory body will find an electronic signature to
be insufficient to comply with its requirements. If such a body
determines that a signature is subject to repudiation, it may well rule
it to be inadequate for legal purposes.

### Electronic Signature Technologies

Recall that the general definition of an electronic signature, “an
electronic sound, symbol, or process, attached to or logically
associated with a contract or other record and executed or adopted by a
person with the intent to sign the record,” does not specify any
particular technology. This can include anything from an image of the
individual’s pen and ink signature added to a document, to an entry in a
database controlled at the application level indicating a specific
user’s action with intent to sign a record, to a cryptographically
strong digital signature.

Unlike the term “electronic signature,” the term “digital signature” has
specific technical implications. The U.S. Digital Signature Standard
describes digital signatures in specific cryptographic terms, “A digital
signature is represented in a computer as a string of binary digits. A
digital signature is computed using a set of rules and a set of
parameters such that the identity of the signatory and integrity of the
data can be verified. An algorithm provides the capability to generate
and verify signatures. Signature generation makes use of a private key
to generate a digital signature. Signature verification makes use of a
public key which corresponds to, but is not the same as, the private
key. Each user possesses a private and public key pair. Public keys are
assumed to be known to the public in general. Private keys are never
shared. Anyone can verify the signature of a user by employing that
user's public key. Signature generation can be performed only by the
possessor of the user’s private key.”[^9] The 1998 version of the DSS
lists two algorithms for creation of digital signatures, DSA and RSA.
Other algorithms, such as elliptic curves are emerging that also fit the
definition.[^10]

Digital signatures are widely held to be the technically best form of
electronic signature. In fact, many analyses begin with the assumption
that electronic signatures will be implemented using some form of
digital signature. The American Bar Association guidelines briefly
acknowledge the existence of simpler electronic signatures but limit
their discussion to digital signatures. The ASTM Standard Guide for
Electronic Authentication of Health Care Information uses language to
extend its consideration to more than digital signatures. “While most
electronic signature standards in the banking, electronic mail, and
business sectors address only digital signature systems, this standard
acknowledges the efforts of industry and systems integrators to achieve
authentication with other methods. Therefore, this standard will not be
restricted to a single technology.”[^11] But it then goes on to discuss
the implementation of digital signatures as the only technology
currently available to meet all the stated requirements. Biometrics are
discussed as providing many of the requirements.[^12]

The Office of Management and Budget (OMB) uses a similar approach to
technology neutrality in its guidance on implementation of the
Government Paperwork Elimination Act, “We do not believe it would be
appropriate to endorse one technology, and we share the concerns of
those commenters who argued against such an endorsement. At the same
time, we recognize that cryptographically-based digital signatures
(i.e., public key technology) hold great promise for ensuring both
authentication and privacy in networked interactions, and may be the
only technology available that can foster interoperability across
numerous applications. There are, however, applications where personal
identification numbers (PINs) and other shared secret techniques may
well be appropriate. These are generally relatively low risk
applications where interoperability is of lesser importance.”[^13]

#### Digital Signature:

Digital signatures use well established standard algorithms in
conjunction with a public/private key pair to create a cryptographically
verifiable signature that authenticates both the signer and the
document. All one needs to verify the signature is a copy of the signed
document, and a reliable copy of the signer’s public key. The public key
is traditionally obtained from a digital certificate of one kind or
another. Two forms predominate. Pretty Good Privacy (PGP) uses a model
in which certificates are signed by other trusted users producing a web
of trust. In its simplest form PGP can be used in a direct trust model
where one only trusts certificates obtained directly from the private
key holder. Public Key Infrastructure (PKI) uses trusted authorities
called Certificate Authorities (CA) to sign certificates for subscribers
after applying a degree of authentication that depends on the specific
policies of the CA. To completely validate a digital signature, one must
apply one additional validation step. At the time the signature is
applied, the certificate must be valid. There must be no evidence that
the private key has been compromised or that the certificate contents
are otherwise invalid. Proof of a signature’s authenticity will
therefore involve producing some sort of proof that the validity of the
certificate was checked at the time the certificate was verified.
Depending on the system used, this may present a point of failure for
many digital signature systems.

#### Digitized Signature:

Perhaps the simplest form of electronic signature to understand is the
simple capture of an image of the signer’s pen and ink signature. This
is used in various applications including signing for package deliveries
on an electronic clipboard carried by the driver, and signing credit
card receipts on a tablet at the checkout line. These do a good job of
replicating the ceremonial aspects of a signature and a reasonable job
of signer authentication. By itself it does nothing to provide data
integrity or binding of the signature to the document signed. Digitized
signatures lack some features of pen and ink signatures making them much
weaker than their old-fashioned counterparts. Most significantly they
lack the pen, ink, and paper. As mentioned earlier, the potential to
perform chemical and physical analysis of a pen and ink signature
provides much of the protection against forgery or duplication of
signature. Copying a digitized signature is a very simple matter and,
without additional controls, the copy is indistinguishable from the
original.

#### Countersigned Digitized Signature:

One reason for using an electronic signature such as a digitized
signature instead of a digital signature is the need, in digital
signature, for the signer to have a public/private key pair. For
applications such as package receipt or even patient consent, this may
not be practical. Many of the same benefits of digital signature can be
provided by having the signer apply a digitized signature and following
this by a witness applying a digital signature computed over the
document and the digitized signature. It is much more practical to
provide intended witnesses with public/private key pairs. The digitized
signature provides the ceremony and authentication of the original
signer. The digital countersignature provides the data integrity as well
as authentication of the witness. If the “witness” is a simple computer
process rather than a human, this approach can still provide the data
integrity function without the countersignature step adding any work at
the user interface level.

#### Proprietary eSignatures:

Proprietary solutions are available to provide varying degrees of
stronger user authentication and binding of the document to the
signature. One such proprietary solution, PenOp captured a signature on
a digitizing pad and used proprietary technology to bind it to the
document being signed. In this case, the signature on the digitizer was
used both as a biometric device for authenticating the user to a
“signature card” database, but also an image to be presented when the
signature was verified. While there were some issues with both false
positive and false negative biometric authentication, the real issue
with this solution was the proprietary nature of the binding. Signatures
could only be verified using the PenOp system and any unknown security
flaws were just that, unknown, because the technology used in the
binding was not open to review as are the cryptographic algorithms used
in digital signature. PenOp no longer exists as far as I can find but
similar proprietary tools will likely always be around and are
appropriate in _some_ situations.

#### Digital X:

There is no formal name for this class of applications, generally known
simply as “electronic signatures.” Here let’s call them “Digital X” to
make a point about their applicability. Just as a person who cannot
write may make a mark to indicate agreement with a document, various
forms of printed or electronic marks can be used to indicate electronic
signature on a document. Just as with the written X, the strength in
these systems arises not from the mark itself, but from the systems
around it. These are the electronic equivalent of the witnesses to the
placement of the written X. Any number of implementations can be
imagined. One might be a closed clinical record system that uses a “sign
this entry” button to make an entry in a database reading, “This entry
was signed by \[Dr. X\] on \[date\].” The database entry by itself
provides essentially no non-repudiation. If the system employs a strong
form of user authentication and includes controls to prevent record
manipulation by other than the controlled system, the overall signature
may be strong enough for its intended use. The countersigned digitized
signature discussed above is an example of such a strong system.

### Regulatory Environment

A thorough analysis of recent legislation and regulations involving
electronic signatures would constitute a major work in its own right and
is beyond the scope of this paper. The following briefly lists some of
the laws and regulations directing the use of electronic signatures in
healthcare.

#### Government Paperwork Elimination Act:

Signed in October of 1998 this act amended 44 USC 3504 to require
federal agencies to provide electronic alternatives to paper
interactions with the government. The essence of the act is captured in
section 1707, “Electronic records submitted or maintained in accordance
with procedures developed under this title, or electronic signatures or
other forms of electronic authentication used in accordance with such
procedures, shall not be denied legal effect, validity, or
enforceability because such records are in electronic form.”

#### Electronic Signatures in Global and National Commerce Act (E-Sign):

This act, which became effective October 1, 2000, gives electronic
signatures and records legal equality with their manual and paper
counterparts. It is technology neutral and even prevents agencies from
requiring specific technologies. It applies to federal agencies and to
contracts and records that affect interstate commerce.

#### Uniform Electronic Transactions Act (UETA):

This model law was developed and recommended to the states by the
National Conference of Commissioners on Uniform State Laws in 1999. If
adopted in the states, it would essentially extend E-Sign to state
agencies and to commercial transactions on a state level. E-Sign
includes provisions to preempt nonconforming versions of UETA. One way a
state implementation of UETA might be nonconforming is by giving greater
effect to a specific technology or technical specification. A
Connecticut version of UETA was proposed in this legislative session but
did not pass.

#### Health Insurance Portability and Accountability Act (HIPAA):

In August of 1998, the US Department of Health and Human Services (HHS)
released a notice of proposed rulemaking containing proposed rules
covering security of electronic transactions as required under the
administrative simplification provisions in HIPAA. Included in these
proposed rules were provisions for electronic signatures. While none of
the specific electronic transactions proposed to date require a
signature, the rules would apply to any covered transactions that
required signature. The proposed rules did not require any specific
technology, but the requirements were strict enough that implementation
would have only been practicable using digital signature. HHS received
many comments to the effect that the rules as written were not practical
given the current state of technology and standards. As a result,
specific e-signature requirements were never (up to 2024) included in
HIPAA rules and healthcare organizations are on their own to
implement—and defend—electronic signatures according to E-Sign as they
see fit.

### Conclusions

Replacement of paper records and communications with electronic
processes offers many opportunities to improve the efficiency and safety
of healthcare. Replacement of pen and ink signatures with their
electronic counterparts offers some opportunities to improve the
security and verifiability of electronic records, but also presents some
serious challenges.

Pen and ink signatures are well accepted in our society and there is a
significant body of law around them. They fulfill some very specific
legal requirements for evidence, ceremony, approval, and efficiency and
logistics. Their electronic counterparts, being much newer, are not as
firmly established. Current legislation and regulations have recognized
the enforceability of electronic signatures but have avoided addressing
the technological requirements.

Many technologies exist for implementation of electronic signatures.
These technologies vary widely in their security, expense, and ease of
use. The selection of appropriate technologies for each application can
only be made in the light of a thorough and well thought out risk
assessment. Appropriate industry standards will help improve the
availability and interoperability of workable electronic signature
solutions.

[^1]: It is not, however, an ANSI accredited standards development organization.
[^2]: Standards for the Internet started out as “Requests For Comment” back in its experimental days and the acronym RFC stuck.
[^3]: Information Security Committee Electronic Commerce and Information Technology Division Section of Science and Technology American Bar Association, _Digital Signature Guidelines; Legal Infrastructure for Certification Authorities and Secure Electronic Commerce_, American Bar Association, 1996, pp 4-6.
[^4]: Not all societies follow the same conventions. In Japan, for example, a handwritten signature does not carry legal status. To legally sign a document, an individual must obtain and register a "Han" stamp. This intricately carved stamp is made from the end grain of a small piece of bamboo. The combination of the grain of the wood and the carving make each stamp unique and unduplicatable. Thus, the ability to apply a legally binding signature depends, in Japan, on possession of a specific item, the individual’s stamp. These stamps are very closely guarded personal possessions.
[^5]: _Electronic Signatures in Global and National Commerce Act_, Sec 106, (5).
[^6]: U.S. Department of Justice, _Legal Considerations In Designing And Implementing Electronic Processes: A Guide For Federal Agencies_, November 2000, p19.
[^7]: ibid. p35.
[^8]: Schneier, B, “Why Digital Signatures Are Not Signatures”, Crypto-Gram, November 15, 2000, Counterpane Internet Security, Inc.
[^9]: U.S. Department Of Commerce/National Institute of Standards and Technology, _Digital Signature Standard (DSS)_, Federal Information Processing Standards Publication 186-1, 1998 December 15, p 1.
[^10]: 2024 additional note: The advent of quantum computing has the computer security community, including the National Institute of Standards and Technology (NIST) working to develop newer, “quantum resistant” algorithms such as those based on multi-dimensional latices.
[^11]: American Society For Testing And Materials, _Standard Guide for Electronic Authentication of Health Care Information_, E1762, p 3.
[^12]: ibid. pp 8-13.
[^13]: Office Of Management And Budget, _Implementation of the Government Paperwork Elimination Act_, available at http://clinton4.nara.gov/OMB/fedreg/gpea2.html. (Now at: https://obamawhitehouse.archives.gov/omb/fedreg_gpea2/)
