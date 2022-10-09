---
date: "2010-06-23T22:02:10Z"
tags:
  - c++
  - qt
title: "Qt : QMultiHash and QHashIterator"
layout: "../../layouts/BlogPost.astro"
---

Qt's implementation of QMultiHash iterator is a little non-intuitive.

One would expect a multi hash to be implemented as `<uniqueKey, list of values>` pairs, where the list of values could even be a set. But QMultiHash is derived from QHash, and it stores the same key multiple times. It stores a number of _unique_ **`key, value`** pairs.

This can cause some amount of confusion while iterating over a multi-hash with a QHashIterator.

Here is what I'd been doing (in-correct code)

```cpp
QHashIterator<Key, Value> it( multiHash );
while( it.hasNext() ) {
    it.next();
    QList<Value> values = multiHash.values( it.key() );
    foreach( const Value &amp; v, values ) {
        // Do whatever with v
    }
}
```

This resulted in the same key being iterated a number of times, which isn't what I wanted.

This was the correct way of doing it -

````cpp
QList<Key> keys = multiHash.uniqueKeys();
foreach( const Key &amp; k, keys ) {
    QList<Value> values = multiHash.values( it.key() );
    foreach( const Value &amp; v, values ) {
        // Do whatever with v
    }
}
```cpp

This was rather obvious once I understood the way it stores a QMultiHash.
````
