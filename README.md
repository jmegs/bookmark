# Bookmark

> _this project is following [README Driven Development][rdd], where the app's features are written out before anything is implemented. Sometimes *way* before..._ 

The goal of bookmark is to be a single bucket where we save things encountered on the internet that we'd like to **remember**. It is not intended to be a 'read later' service like [Pocket][pocket] – articles read in a Pocket type service that we want to **remember** would in turn be saved into bookmark.

What sets bookmark apart from other services is that it focuses on categorizing items by **what they are** rather than **what they're about**. This is an inversion of the "moodboard" approach of tools like Pinterest which ask "what topic should we file this under". In my experience, the Pinterest model doesn't match up with how the mind recalls information. When we want to recall something we saw online we think "I read an amazing **article** last week about **baseball**, where the hell was it?". We focus on the **type** of thing it was before it's topic. If this is true, the Pinterest approach — "Show me **everything** I ever saved about **baseball**" — isn't the most efficient way to recall our specific article.

The tool that comes closest to this principle right now is [mymind], which also prioritizes frictionless capture and many media types. But, by relying solely on automatic tagging and text search, it doesn't have a good hit rate on tagging **what the item _is_**. I find myself fighting the system to find "articles I've saved", because the "AI" doesn't tag every article as such.

The same thing happens with a manually tagged approach a la [Pinboard]. Without a tightly defined, cannonical set of tags for media **types**, search becomes more prone to error.

So, bookmark seeks a middle way between the category based moodboard and the flat tagged list aproaches.

## Concepts

### Item

The core element of bookmark is the `Item`. This is a single piece of information from the internet that we'd like to file away and remember.

### Type

Each `Item` is assigned a `Type` when it is captured. The `Type` describes **what kind of thing the item is**. Types are inferred based on a set of rules that take into account domain name, MIME type, document metadata, etc.

| Type       | Description                                                       |
| ---------- | ----------------------------------------------------------------- |
| Image      | A bitmap or vector image that's saved directly to bookmark        |
| GIF        | A gif saved directly to bookmark                                  |
| Video      | A link from known video hosts like YouTube, or a direct video url |
| Screenshot | Special type, website screenshots captured by bookmark extension  |
| Article    | The default type for a generic webpage                            |
| Quote      | Plain text directly saved into bookmark                           |
| Code       | Any URL from a known source code host like Github                 |
| Recipe     | Any URL from a known source of recipes, like bonappetit.com       |
| Book       | A URL from a book listing on amazon.com                           |
| Place      | A url from a known location service, like google maps or yelp.    |
| Song       | A URL from a known music provider, like Spotify.                  |
| Product    | a URL that appears to be a product for sale.                      |

### Collection

Users of bookmark can create up to a limit TBD of `Collections`, that hold items regardless of their `Type`. Items can be part of multiple Collections. The number of active collections is intentionally limited to prevent over-organization and reduce the potential for stale content. Collections can be archived at any time, preserving the grouping of `Item`s for reference.

This borrows from Tiago Forte's [PARA method][para], specifically the concepts of Resource — "a topic or theme of ongoing interest" — and Archive — items that "have become inactive", "you are no longer committed to maintaining", or "are no longer iterested in".

In addition to user-defined collections, there are several "system" collections:

| Collection    | Description                                  |
| ------------- | -------------------------------------------- |
| Everything    | All items, regardless of type                |
| Uncategorized | All items that do not belong to a collection |
| Visuals       | Image OR GIF OR Video OR Screenshot          |
| Words         | Article OR Quote                             |

### Query

We browse saved `Item`s by constructing a `Query` of the format `Show me <TYPE> in <COLLECTION>`.

- Show me `GIFs` in `Everything`
- Show me `Visuals` in `Sci-Fi`
- Show me `Products` in `Everything`
- Show me `Everything` in `Sports`

## How it Works

1. User captures an Item via browser extension, iOS shortcut, or directly via url
	- browser extension has option to save page url or screenshot of page
	- on capture, if the user has collections defined, they can assign the item to a collection

2. bookmark parses the item and assigns it a `type`
	- the parser will contain hard coded rulesets that flag certain urls or content types, for example, anything from a list of known recipe domains will be assigned type of recipe.

3. the item is saved as a document in the database. each `type` has it's own needed fields. If the user assigned it to a collection on capture, it is also associated with that collection

4. on the client, an interface lets users construct `<TYPE> in <COLLECTION>` queries. Those queries will call the db and retrive items where the type and collection fields match.

5. From the UI, the user can
	- view an item in a single item view
	- assign an item to one or more collections
	- remove an item from a collection
	- create a collection (up to the limit TBD)
	- archive a collection

[rdd]: https://tom.preston-werner.com/2010/08/23/readme-driven-development.html
[pocket]: https://getpocket.com
[mymind]: https://mymind.com/
[para]: https://fortelabs.co/blog/para/
[pinboard]: https://pinboard.in
