---
layout: default
title: ZeeQL - CoreData like Swift ORM
tags: swift orm database server side postgresql sqlite
---

<p>
  <img src="https://img.shields.io/badge/swift-3-blue.svg" />
  <img src="https://img.shields.io/badge/swift-4-blue.svg" />
  <img src="https://img.shields.io/badge/os-macOS-green.svg?style=flat" />
  <img src="https://img.shields.io/badge/os-iOS-green.svg?style=flat" />
  <img src="https://img.shields.io/badge/os-tuxOS-green.svg?style=flat" />
  <img src="https://travis-ci.org/ZeeQL/ZeeQL3.svg?branch=master" />
</p>

**ZeeQL**
is a Swift ORM / database access library primarily inspired by EOF, 
and in consequence CoreData, adding some ActiveRecord concepts.
The framework is self contained and doesn't have any 3rd party dependencies.
It comes with a SQLite3 driver builtin,
and APR as well as a standalone PostgreSQL driver as optional modules.

The framework can be used in either server side Swift applications or
in client applications (iOS or macOS apps).
It is not quite there yet, but could potentially serve as a pure Swift
CoreData replacement that doesn't just work w/ SQLite3 but with other
servers as well.

### Provides Choices

Use raw SQL queries in a typesafe way:

```swift
try adaptor.select("SELECT name, count FROM pets") {
  ( name: String, count: Int) in
  print("\(name): #\(count)")
}
```

Use full object fetches, and prefetch relationships:

```swift
let objects = try datasource.fetchObjects(
  Person
    .where(Person.e.login.like("he*"))
    .limit(10)
    .prefetch(Person.e.addresses)
    .order(by: Person.e.login)
)
```

Or partial object fetches, which still provide most of the neat stuff
while avoiding fetching full objects all the time:

```swift
let partials = try db
  .select(Person.e.firstname, from: Persons.self)
   .where(Person.e.login.like("he*"))
   .order(by: Person.e.login)
)
```

Declare your models in neat code (either typesafe, or by using names),
or fetch them from the database information schema,
or design them with the Xcode CoreData modeler.
  
### WORK IN PROGRESS, STAY TUNE

ZeeQL is still being prepared. Please stay tuned for the release.
If you want to stay up to date, subscribe to the
[ZeeQL Blog](/blog/)

### Status

Available:

- Simple KVC implementation
- Control Layer
  - Qualifiers and qualifier parser (NSPredicate's in CD)
  - SortOrdering's etc
- Access Layer
  - Adaptors
    - SQLite3, native
    - APR Adaptors (Apache Portable Runtime, kinda a mini-ODBC),
      w/ reflection for PG&amp;SQLite
    - PostgreSQL, libpq based<
  - ActiveDataSource / ActiveRecord
          (tracks snapshot in the object, instead of using the EditingContext)
  - Typesafe, simple SQL selects
  - or, ORM style mapped objects
  - Model
    - Schema reflection for SQLite3 and PG, change tagging
    - Neat in-code model/entity setup
    - CoreData model loading
    - FancyModelMaker - take a proper SQL database schema and turn it to a
      nice camelCase schema.
      Surprisingly great against existing databases.
  - Partial object fetches
  - Prefetching of relationships
  - Qualifier SQL generation supporting relationship traversal:
    `owner.address.street LIKE '*way'`,
     qualifier variables, and more.
  - Embedding of raw SQL in qualifiers
  - Recursive relationship prefetches (`comments*`)

Missing:

- Proper EditingContext
- Codable support (work in progress)
- Faults
- Extensive Documentation
- KVC generator
- ... many small things :-)

<div class="posts">
  {% for post in site.posts %}
    <article class="post">

      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>

      <div class="entry">
        {{ post.excerpt }}
      </div>
      
      <div class="date">
        <table border="0" width="100%"> <!-- old skool -->
          <tr>
            <td>{{ post.date | date: "%B %e, %Y" }}</td>
            <td style="text-align:right;"><a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a></td>
          </tr>
        </table>
      </div>
    </article>
  {% endfor %}
</div>
