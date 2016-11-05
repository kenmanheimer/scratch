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
        <ul> Home info
          <ul> Email
            <ul> <strong>cith@home.example.net</strong> </ul>
          </ul>
          <ul> Phone
            <ul> <strong>555-1234</strong> </ul>
          </ul>
        </ul>
      </ul>
    </td>
    <td>
      <ul> <strong>CitH</strong>
        <ul> Mom's Place, Inc. directory info
          <ul> Email
            <ul> <strong>cith@momsplc.example.com</strong> </ul>
          </ul>
          <ul> Phone
            <ul> <strong>555-9101</strong> </ul>
          </ul>
        </ul>
      </ul>
    </td>
    <td>
      <ul> <strong>Thing1</strong>
        <ul> Mom's Place, Inc. directory info
          <ul> Email
            <ul> <strong>thing1@momsplc.example.com</strong> </ul>
          </ul>
          <ul> Phone
            <ul> <strong>555-9102</strong> </ul>
          </ul>
        </ul>
      </ul>
    </td>
  </tr>
</table>

There are many equivalent ways for this information to be composed with spherical. One follows.

`Cat in the Hat`'s personal info:

```
    >>> cith = Item("Cat in the Hat")
    >>> cith_home = cith.add(Item("home"))
    >>> cith_email = cith_home.add(Item("email"))
    >>> cith_phone = cith_home.add(Item("phone"))
    >>> cith_email.add(Item("cith@home.example.net"))
    >>> cith_phone.add(Item("555-1234"))
    >>> str(cith['home']['email'].contents()) == str(cith_email.contents())
    True
```

`Mom's Place, Inc.`, an employer:

```
    >>> momsplc = Item("Mom's Place, Inc.")
    >>> momsplc_emps = momsplc.add(Item("employees"))
```

`Mom's Place, Inc.` employee info, for `Cat in the Hat` and `Thing 1`

```
    >>> cith_momsplc = momsplc_emps.add(Item("CitH"))
    >>> cith_momsplc_email = cith_momsplc.add(Item("email"))
    >>> cith_momsplc_email.add(Item("cith@momsplc.example.com"))
    >>> cith_momsplc_phone = cith_momsplc.add(Item("phone"))
    >>> cith_momsplc_phone.add(Item("555-9101"))
    >>> str(cith['momsplc']['phone'].contents()) ==
        str(cith_momsplc_phone.contents())
    True
    >>> tng1_momsplc = momsplc_emps.add(Item("Thing 1"))
    >>> tng1_momsplc_email = tng1_momsplc.add(Item("email"))
    >>> tng1_momsplc_email.add(Item("tng1@momsplc.example.com"))
    >>> tng1_momsplc_phone = tng1_momsplc.add(Item("phone"))
    >>> tng1_momsplc_phone.add(Item("555-9102"))
    >>> str(tng1['momsplc']['email'].contents()) ==
        str(tng1_momsplc_email.contents())
    True
```

Now, `Cat in the Hat` can register the employment:

```
    >>> cith_work = cith.add(Item("work"))
    >>> cith_at_momsplc = cith_work.add(momsplc.employees
```

Supposing `Cat in the Hat` has access to their `Mom's Place` employee info, they can maintain a directory of their various address info:




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

* Mom's Place
    * ∋ Directory ⊇ [Mom'sPlc/CitH/Directory] ∪ [Mom'sPlc/Thing1/Directory]
        * ∋ Email ⊇ [Mom'sPlc/CitH/Directory/Email] ∪ [Mom'sPlc/Thing1/Directory/Email]
            * ∋ cith@momsplc.example.com
            * ∋ thing1@momsplc.example.com
        * ∋ Phone ⊇ [Mom'sPlc/CitH/Directory/Phone] ∪ [Mom'sPlc/Thing1/Directory/Phone>
            * ∋ 555-4321
            * ∋ 555-7890
