Describing an UBER Document
===========================

## Original UBER Document

This is pulled from the [UBER documentation](https://rawgithub.com/mamund/media-types/master/uber-hypermedia.html#_json_example).

```json
{ 
  "uber" :
  {
    "version" : "1.0",
    "data" :
    [
      {"rel" : ["self"], "url" : "http://example.org/"},
      {"rel" : ["profile"], "url" : "http://example.org/profiles/people-and-places"},
      
      {
        "data" : 
        [
          {
            "id" : "people", 
            "rel" : ["collection","http://example.org/rels/people"], 
            "url" : "http://example.org/people/",
            "data" : 
            [
              {
                "name" : "create", 
                "rel" : ["http://example.org/rels/create"], 
                "url" : "http://example.org/people/",
                "model" : "g={givenName}&f={familyName}&e={email}",
                "action" : "append"
              },
              {
                "name" : "search",
                "rel" : ["search","collection"],
                "url" : "http://example.org/people/search",
                "model" : "?g={givenName}&f={familyName}&e={email}"
              },
              {
                "name" : "person",
                "rel" : ["item","http://example.org/rels/person"],
                "url" : "http://example.org/people/1",
                "data" :
                [
                  {"name" : "givenName", "value" : "Mike"},
                  {"name" : "familyName", "value" : "Amundsen"},
                  {"name" : "email", "value" : "mike@example.org"},
                  {"name" : "avatarUrl", "transclude" : "true", 
                      "value" : "http://example.org/avatars/1", 
                      "accepting" : ["image/*"]
                  }
                ]
              },
              {
                "name" : "person",
                "rel" : ["item","http://example.org/rels/person"],
                "url" : "http://example.org/people/2",
                "data" :
                [
                  {"name" : "givenName", "value" : "Mildred"},
                  {"name" : "familyName", "value" : "Amundsen"},
                  {"name" : "email", "value" : "mildred@example.org"},
                  {"name" : "avatarUrl", "transclude" : "true", 
                      "value" : "http://example.org/avatars/2", 
                      "accepting" : ["image/*"]
                  }
                ]
              }
            ]
          },
          
          {
            "id" : "places", 
            "rel" : ["collection","http://example.org/rels/places"], 
            "url" : "http://example.org/places/",
            "data" :
            [
              {
                "name" : "search",
                "rel" : ["search","collection"],
                "url" : "http://example.org/places/search",
                "model" : "?r={addressRegion}&l={addressLocality}&p={postalCode}"
              },
              {
                "name" : "place",
                "rel" : ["item","http://example.org/rels/place"],
                "url" : "http://example.org/places/a",
                "data" : 
                [
                  {
                    "name" : "name", 
                    "value" : "Home"
                  },
                  {
                    "name" : "address",
                    "data" :
                    [
                      {"name" : "streetAddress", "value" : "123 Main Street"},
                      {"name" : "addressLocalitly", "value" : "Byteville"},
                      {"name" : "addressRegion", "value" : "MD"},
                      {"name" : "postalCode", "value" : "12345"}
                    ]
                  }
                ]
              },
              {
                "name" : "place",
                "rel" : ["item","http://example.org/rels/place"],
                "url" : "http://example.org/places/b",
                "data" : 
                [
                  {
                    "name" : "name", 
                    "value" : "Work"
                  },
                  {
                    "name" : "address",
                    "data" : 
                    [
                      {"name" : "streetAddress", "value" : "1456 Grand Ave."},
                      {"name" : "addressLocalitly", "value" : "Byteville"},
                      {"name" : "addressRegion", "value" : "MD"},
                      {"name" : "postalCode", "value" : "12345"}
                    ]
                  }
                ]
              }
            ]
          }
        ]
      }
    ]    
  }
}
```

## Described with Hyperdescribe

```json
{
  "meta":{
    "links":[
      {
        "rel":["self"],
        "url":"http://example.org/"
      },
      {
        "rel":["profile"],
        "url":"http://example.org/profiles/people-and-places"
      }
    ]
  },
  "resources":[
    {
      "id": "people",
      "rel":["collection","http://example.org/rels/people"],
      "url":"http://example.org/people/",
      "resources":[
        {
          "name": "person",
          "property": "person",
          "rel":["item","http://example.org/rels/person"],
          "url":"http://example.org/people/1",
          "properties": [
            { "name": "givenName", "value": "Mike" },
            { "name": "familyName", "value": "Amundsen" },
            { "name": "email", "value": "mike@example.org" }
          ],
          "links": [
            {
              "property": "avatarURL",
              "href": "http://example.org/avatars/1",
              "transclude": true,
              "types": ["image/*"]
            }
          ]
        },
        {
          "name": "person",
          "property": "person",
          "rel":["item","http://example.org/rels/person"],
          "url":"http://example.org/people/2",
          "properties": [
            { "name": "givenName", "value": "Mildred" },
            { "name": "familyName", "value": "Amundsen" },
            { "name": "email", "value": "mildred@example.org" }
          ],
          "links": [
            {
              "property": "avatarURL",
              "href": "http://example.org/avatars/2",
              "transclude": true,
              "types": ["image/*"]
            }
          ]
        }
      ],
      "actions":[
        {
          "name":"create",
          "url":"http://example.org/people/",
          "rel":["http://example.org/rels/create"],
          "method":"POST",
          "fields":[
            {
              "name":"g",
              "type":"text",
              "mapsTo": "givenName"
            },
            {
              "name":"f",
              "type":"text",
              "mapsTo": "familyname"
            },
            {
              "name":"e",
              "type":"text",
              "mapsTo": "email"
            }
          ]
        },
        {
          "name":"search",
          "url":"http://example.org/people/search",
          "rel": ["search","collection"],
          "method":"GET",
          "fields":[
            {
              "name":"g",
              "type":"text",
              "mapsTo": "givenName"
            },
            {
              "name":"f",
              "type":"text",
              "mapsTo": "familyname"
            },
            {
              "name":"e",
              "type":"text",
              "mapsTo": "email"
            }
          ]
        }
      ]
    },
    {
      "id":"places",
      "rel":["collection","http://example.org/rels/places"],
      "url":"http://example.org/places/",
      "resources":[
        {
          "name":"place",
          "rel":["item","http://example.org/rels/place"],
          "url":"http://example.org/places/a",
          "properties": [
            { "name": "name", "value": "Home" }
          ],
          "resources": [
            {
              "name":"address",
              "property":"address",
              "properties":[
                { "name": "streetAddress", "value": "123 Main Street" },
                { "name": "addressLocalitly", "value": "Byteville" },
                { "name": "addressRegion", "value": "MD" },
                { "name": "postalCode", "value": "12345" }
              ]
            }
          ]
        },
        {
          "name":"place",
          "rel":["item","http://example.org/rels/place"],
          "url":"http://example.org/places/b",
          "properties": [
            { "name": "name", "value": "Work" }
          ],
          "resources": [
            {
              "name":"address",
              "property":"address",
              "properties":[
                { "name": "streetAddress", "value": "1456 Grand Ave." },
                { "name": "addressLocalitly", "value": "Byteville" },
                { "name": "addressRegion", "value": "MD" },
                { "name": "postalCode", "value": "12345" }
              ]
            }
          ]
        }
      ],
      "actions":[
        {
          "name":"search",
          "url":"http://example.org/places/search",
          "rel":["search","collection"],
          "method":"GET",
          "fields":[
            {
              "name":"r",
              "type":"text",
              "mapsTo": "addressRegion"
            },
            {
              "name":"l",
              "type":"text",
              "mapsTo": "addressLocality"
            },
            {
              "name":"p",
              "type":"text",
              "mapsTo": "postalCode"
            }
          ]
        }
      ]
    }
  ]
}
```
