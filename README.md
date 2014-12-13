CPHL
====

CPHL, or the Cross-Platform Hypertext Language is a hypertext specification for RESTful APIs.  CPHL is based on HAL, but is designed to allow for greater flexibility, transmission of additional information, and empowerment of resource specific code on demand.

CPHL requires specific key value pairs that can then be distributed across different platforms, such as JSON, XML, and YAML (see examples in repository).

<h3>General Rules</h3>
CPHL is designed to be as flexible as possible, but does recommend the use of both the _definition and _links collections, although at minimum the _links collection is required.

Based on HAL, CPHL is nestable meaning that you can utilize it for actions specific to the entire collection or singular result, or nest it within each item of the collection for that item's specific actions, or both.


<h3>The Specification</h3>
<h4>_definition</h4>
_definition contains the collection of API definitions, as described by WADL, RAML, Swagger, or API Blueprint.

The _definition collection breaks down into key value pairs where the name of the spec (camelCase, no spaces) represents the key, and the URL to the file containing the API defintion is the value.

```
"_definition" : {
  "raml" : "http://api.domain.com/docs/api/raml",
  "swagger" : "http://api.domain.com/docs/api/swagger"
}
```

<h4>_links</h4>
_links contains the hypertext links collection utilizing the action as the key.

Each individual link contains the following structure (optional constraints italic):
- *title*: the title of the action (eg: Edit User)
- *description*: a brief description of the action
- <b>href</b>: the URI for the client to achieve the action
- *methods*: an array of available methods to perform this specific action (eg edit: put, patch; view: get)
- *formats*: an array of formats that this action supports (ie JSON, XML, etc)
  - *{formatName}*
    - *mimeType*: the mime-type of the format (eg: application/json)
    - *schema*: the URL to the schema for this format
- *docHref*: the URL to online documentation for this action
- *code*: an array of available code libraries for this action
  - *{language}*: the URL of the code library accessible to the client
  
<h3>Examples</h3>
<h4>application/cphl+json</h4>
```
"_definition" : {
  "raml" : "http://api.domain.com/docs/api/raml",
  "swagger" : "http://api.domain.com/docs/api/swagger"
}
 
"_links" : {
  "edit" : {
    "title" : "Edit User",
    "description" : "edit the user",
    "href" : "/api/resource",
    "methods" : ["put", "patch"],
    "formats" : {
        "json" : {
          "mimeType" : "application/json",
          "schema" : "http://api.domain.com/docs/api/editSchema.json"
        },
        "xml" : {
          "mimeType" : "text/xml",
          "schema" : "http://api.domain.com/docs/api/editSchema.xml"
        },
    },
    "docHref" : "http://api.domain.com/docs/edit",
    "code" : {
        "php" : "http://code.domain.com/phplib/edit.tgz",
        "java" : "http://code.domain.com/javalib/edit.tgz",
        "ruby" : "http://code.domain.com/rubylib/edit.tgz",
    }
  }
}
```

<h4>application/cphl+xml</h4>
```
<_definition>
  <raml>
    <href>http://api.domain.com/docs/api/raml</href>
  </raml>
  <swagger>
    <href>http://api.domain.com/docs/api/swagger</href>
  </swagger>
</_definition>
 
<_links>
  <edit>
    <title>Edit User</title>
    <description>edit the user</description>
    <href>/api/resource</href>
    <methods>put</methods>
    <methods>patch</methods>
    <formats>
      <json>
        <mimeType>application/json</mimeType>
        <schema>http://api.domain.com/docs/api/editSchema.json</schema>
      </json>
      <xml>
        <mimeType>text/xml</mimeType>
        <schema>http://api.domain.com/docs/api/editSchema.xml</schema>
      </xml>
    </formats>
    <formats>xml</formats>
    <docHref>http://api.domain.com/docs/edit</docHref>
    <code>
      <php>http://code.domain.com/phplib/edit.tgz</php>
      <java>http://code.domain.com/javalib/edit.tgz</java>
      <ruby>http://code.domain.com/rubylib/edit.tgz</ruby>
    </code>
  </edit>
</_links>
```

<h4>application/cphl+yaml</h4>
```
_definition:
  raml:
    href: http://api.domain.com/docs/api/raml
  swagger:
    href: http://api.domain.com/docs/api/swagger
    
_links:
  edit:
    title: Edit User
    description: edit the user
    href: /api/resource
    methods:
      - put
      - patch
    formats:
      json:
        mimeType: application/json
        schema: http://api.domain.com/docs/api/editSchema.json
      xml:
        mimeType: text/json
        schema: http://api.domain.com/docs/api/editSchema.xml
    docHref: http://api.domain.com/docs/edit
    code:
      php: http://code.domain.com/phplib/edit.tgz
      java: http://code.domain.com/javalib/edit.tgz
      ruby: http://code.domain.com/rubylib/edit.tgz
```
