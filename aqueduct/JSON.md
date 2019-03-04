

  
  "type": "push",  
  "user": "bob",
  "tags": ["v1"]  
}

The value of  `Event.contents`  would only contain the array for the key "tags":

["v1"]

You may also index arrays in a JSON column using the same subscript operator, and the subscript operator can also be nested. For example, the following query would fetch the "tags" array, and then fetch the string at index 0 from it:

final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["tags"][0]]);
final eventsWithFirstTag = await query.fetchOne();
eventsWithFirstTag.contents.data == "v1";

If a key or index does not exist in the JSON document, the value of the returned property will be null. For this reason, you should use null-aware operators when accessing  `Document.data`:

final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["tags"][7]]); // 7 is out of bounds
final eventsWithFirstTag = await query.fetchOne();
if (eventsWithFirstTag.contents?.data == "v1") {
  ...
}

When fetching elements from a JSON array, you may use negative indices to specify a index from the end of the array.

final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["tags"][-1]]);
final eventsWithLastTag = await query.fetchOne();

Note that you can only fetch a single sub-structure from a  `Document`  column per query. That is, you may not do the following:

// Invalid
final query = Query<Event>(context)
  ..returningProperties((e) => [e.id, e.contents["type"], e.contents["user"]]);

For operations not supported by  `Query<T>`, you may use SQL directly:

final eventTagCounts = await context.persistentStore.execute("SELECT jsonb_array_length(contents->'tags') from _Event");













<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg5MTkxNjEyN119
-->