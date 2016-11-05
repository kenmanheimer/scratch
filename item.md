# Spherical Item Objects

This document describes Spherical Item features, using executable code to demonstrate usage and essential invariants. (Some detailed features specific to particular routines are documented in the routine's docstrings.)  Invoke 'python item.py' to run the tests in this text and those docstrings.

```
>>> from spherical import Item
>>> it = Item("An item")
>>> other = Item("Another item")
```

## Spherical Item, Collectively

1. Every spherical item is addressed by a reference ID `ref` that is unique across all items, local and remote. (The reference IDs are sub-type 1 of [RFC 4122 UUIDs](https://tools.ietf.org/html/rfc4122.html).)

   ```
   >>> it.ref != other.ref != it._collection.ref
   True
   ```
2. Each item's reference ID is contrived to have it's "node" part identical to the node part of the collection, and hence that of all the other items in the collection:

   ```
   >>> it.ref.node == it._collection.ref.node
   True
   ```

   This enables identifying the collection to which an item belongs just from its ref.

3. The collection of all items in a spherical installation has a single, encompassing collection item, that is findable from every item instance.

   ```
   >>> it._collection.ref == other._collection.ref
   True
   ```
4. Every item in the collection is registered as a constituent of the collection object:

   ***

## Composition

Given:

<table>
  <tr>
    <th> Personal CitH </th>
    <th> Mom's Place Employee CitH </th>
    <th> Mom's Place Employee Thing1 </th>
  </tr>
  <tr>
    <td>
      <ul> <strong>CitH</strong>
        <ul> Directory info
          <ul> Email
            <ul> <strong>cith@home</strong> </ul>
          </ul>
          <ul> Phone
            <ul> <strong>555-1234</strong> </ul>
          </ul>
        </ul>
      </ul>
    </td>
    <td>
      <ul> <strong>CitH</strong>
        <ul> Directory info
          <ul> Email
            <ul> <strong>cith@momsplc</strong> </ul>
          </ul>
          <ul> Phone
            <ul> <strong>555-4321</strong> </ul>
          </ul>
        </ul>
      </ul>
    </td>
    <td>
      <ul> <strong>Thing1</strong>
        <ul> Directory info
          <ul> Email
            <ul> <strong>thing1@momsplc</strong> </ul>
          </ul>
          <ul> Phone
            <ul> <strong>555-9876</strong> </ul>
          </ul>
        </ul>
      </ul>
    </td>
  </tr>
</table>

There are many equivalent ways for this information to be composed with spherical. One follows.

```
    >>> cith = Item("Cat in the Hat")
    >>> momsplc = Item("Mom's Place")
    >>> cith_dir = cith.add(Item("directory"))
    >>> cith_work = cith.add(Item("work"))
    >>> cith_at_momsplc = cith_work.add(momsplc)
```

How about viewing a composite directory for all Mom's Place employees?

```
    >>>
```

* CitH ⊇ [Personal/CitH] ∪ [Mom's Place/CitH]
    * ∋ Directory ⊇ [Personal/CitH/Directory] ∪ [Mom'sPlc/CitH/Directory]
        * ∋ Email ⊇
            [Personal/CitH/Directory/Email] ∪ [Mom'sPlc/CitH/Directory/Email]
            * ∋ cith@home.example.net
            * ∋ cith@momsplc.example.com
        * ∋ Phone ⊇
            [Personal/CitH/Directory/Phone] ∪ [Mom'sPlc/CitH/Directory/Phone]
            * ∋ 555-1234
            * ∋ 555-4321
