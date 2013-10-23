---
layout: api-command 
language: Ruby
permalink: api/ruby/difference/
command: difference 
github_doc: https://github.com/rethinkdb/docs/edit/master/2-query-language/api/ruby/document-manipulation/difference.md
related_commands:
---

{% apibody %}
array.difference(array) &rarr; array
{% endapibody %}

Remove the elements of one array from another array.

__Example:__ Retrieve Iron Man's equipment list without boots.

```rb
r.table('marvel').get('IronMan')[:equipment].difference(['Boots']).run(conn)
```

