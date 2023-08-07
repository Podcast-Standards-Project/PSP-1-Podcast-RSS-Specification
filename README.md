
# PSP-1: The Podcast RSS Standard

## Table of Contents

<!-- TOC -->
- [Table of Contents](#table-of-contents)
- [About This Document](#about-this-document)
  - [What is The Podcast Standards Project PSP?](#what-is-the-podcast-standards-project-psp)
  - [Why is PSP Needed?](#why-is-psp-needed)
  - [What is a Podcast?](#what-is-a-podcast)
- [Required RSS Namespace Declarations](#required-rss-namespace-declarations)
  - [HTML and Link Support optional](#html-and-link-support-optional)
- [RSS Feed Elements](#rss-feed-elements)
  - [Required `<channel>` elements](#required-channel-elements)
    - [`<atom:link rel="self">`](#channel-atom)
    - [`<title>`](#channel-title)
    - [`<description>`](#channel-description)
    - [`<link>`](#channel-link)
    - [`<language>`](#channel-language)
    - [`<itunes:category>`](#channel-itunes-category)
    - [`<itunes:explicit>`](#channel-itunes-explicit)
    - [`<itunes:image>`](#channel-itunes-image)
  - [Recommended `<channel>` elements](#recommended-channel-elements)
    - [`<podcast:locked>`](#channel-podcast-locked)
    - [`<podcast:guid>`](#channel-podcast-guid)
    - [`<itunes:author>`](#channel-itunes-author)
  - [Optional supported `<channel>` elements](#optional-supported-channel-elements)
    - [`<copyright>`](#channel-copyright)
    - [`<podcast:txt>`](#channel-podcast-txt)
    - [`<podcast:funding>`](#channel-podcast-funding)
    - [`<itunes:type>`](#channel-itunes-type)
    - [`<itunes:complete>`](#channel-itunes-complete)
  - [Required `<item>` elements](#required-item-elements)
    - [`<title>`](#item-title)
    - [`<enclosure>`](#item-enclosure)
    - [`<guid>`](#item-guid)
  - [Recommended <item> elements](#recommended-item-elements)
    - [`<link>`](#item-link)
    - [`<pubDate>`](#item-pubdate)
    - [`<description>`](#item-description)
    - [`<itunes:duration>`](#item-itunesduration)
    - [`<itunes:image>`](#item-itunes-image)
    - [`<itunes:explicit>`](#item-itunes-explicit)
    - [`<podcast:transcript>`](#item-podcast-transcript)
  - [Optional supported `<item>` elements](#optional-supported-item-elements)
    - [`<itunes:episode>`](#item-itunes-episode)
    - [`<itunes:season>`](#item-itunes-season)
    - [`<itunes:episodeType>`](#item-itunes-episode-type)
    - [`<itunes:block>`](#item-itunes-block)
<!-- /TOC -->

## About This Document

This document specifies requirements and best practices for [PSP](#what-is-the-podcast-standards-project-psp)-certififed podcast feeds. It defines required and optional RSS [namespaces](#required-rss-namespace-declarations) and [feed elements](#rss-feed-elements) that have proven the most useful, and codifies other conventions to remove ambiguities that app creators have wrestled with since the dawn of podcasting.

This specification builds on the [RSS feed format standard](https://www.rssboard.org/rss-specification), which is possible thanks the foresight of the [creators of RSS](https://www.rssboard.org/rss-history).

### What is The Podcast Standards Project (PSP)?

The Podcast Standards Project (PSP) is a grassroots industry coalition to advocate for and help evolve open podcasting. Its mission is to develop modern, open standards, unlock innovation, and improve podcasting for both listeners and creators.

### Why is PSP Needed?

In the beginning, podcasting was “open” by definition — any podcast could be enjoyed via any podcast app, just as any web browser can view any website. In the mid-2010s, podcasting broke through as a mainstream medium.

Big tech companies naturally stepped in to duplicate podcasting’s success with closed, proprietary platforms. [They called shows distributed on their closed platforms “podcasts” to leverage podcasting’s popularity](https://en.wikipedia.org/wiki/Embrace,_extend,_and_extinguish), but the cost was that the word “podcasting” lost its standards-based guarantee. These closed platforms remove choice — they lock audiences into one player, lock creators into a single vendor’s services, and take ownership of the relationship between creators and their audiences.

### What is a Podcast?

PSP supports “open” or “standards-based” podcasting, which is based open web standards and works with any standards-based app or service. Specifically, podcasts are distributed as RSS feeds which represent a collection of episodes with associated audio and/or video content, as well as other useful show and episode metadata.

## Required RSS Namespace Declarations

Podcast feeds utilize RSS 2.0 tags and require extensions from three namespaces (`itunes`, `podcast`, and `atom`). These namespace declarations must be made in the opening of your XML.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd"
xmlns:podcast="https://podcastindex.org/namespace/1.0"
xmlns:atom="http://www.w3.org/2005/Atom">
```

### HTML and Link Support (optional)

To include HTML in your podcast feed, you must also declare the [RDF Site Summary 1.0 Modules: Content](https://web.resource.org/rss/1.0/modules/content/) namespace in the second line of your XML.

Together with the required `<itunes>`, `<podcast>`, and `<atom>` namespace declarations, the first two lines of RSS look like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd"
xmlns:podcast="https://podcastindex.org/namespace/1.0"
xmlns:atom="http://www.w3.org/2005/Atom"
xmlns:content="http://purl.org/rss/1.0/modules/content/">
```

With the RDF Site Summary 1.0 Content Module Specification namespace included, you can enclose all portions of your XML that contain embedded HTML in a CDATA section to prevent formatting issues and to ensure proper link functionality.

```xml
<![CDATA[
  <a href="https://www.podstandards.org">The Podcast Standards Project</a>
]]>
````

Namespace definitions are case-sensitive and must be entered as shown.

Feeds must be plain text UTF-8 encoded. Node values are limited to 255 characters unless otherwise specified and should have no leading or trailing spaces.

## RSS Feed Elements

All elements within a podcast RSS feed will have `<channel>` or `<item>` parent tags. Elements at the `<channel>` level describe the podcast, while elements at the `<item>` level describe an episode.

Required elements must be present in your RSS feed to pass validation. Recommended elements are encouraged because they provide useful information to listeners. Situational elements may be important for specific podcasts, podcasters, or listeners.

**Note:** This document describes the standard elements for podcast RSS feeds. The extendable nature of RSS allows for the use of additional elements as long as a proper namespace extension is declared in the XML. The itunes, podcast, and atom namespace extensions include additional "non-standard" elements that can be used in podcast RSS feeds, although broad support should not be expected.

<!--
  —————————————————————————————
  Required `<channel>` elements
  —————————————————————————————
-->

### Required `<channel>` elements

<!-- <atom:link rel="self"> -->
<a id="channel-atom"></a>

#### `<atom:link rel="self">`

The declared canonical feed URL for the podcast. [Official Docs](https://datatracker.ietf.org/doc/html/rfc4287#section-4.2.7)

````xml
<atom:link href="https://www.podstandards.org/my-podcast.rss"
rel="self" type="application/rss+xml" />
````

<!-- <title> -->
<a id="channel-title"></a>

#### `<title>`

The podcast title. A string containing the name of a podcast and nothing else. [Official Docs](https://cyber.harvard.edu/rss/rss.html#requiredChannelElements)

````xml
<title>Sample Podcast Title</title>
````

_Including keywords in an attempt to improve a podcast's search ranking, may result in being blocked from certain directories._

<!-- <itunes:description> -->
<a id="channel-description"></a>

#### `<description>`

Text that describes a podcast to potential listeners. [Official Docs](https://cyber.harvard.edu/rss/rss.html#requiredChannelElements)

````xml
<description>Sample podcast description.</description>
````

````xml
<description><![CDATA[Sample podcast description.]]></description>
````

The maximum amount of text allowed for this tag is 4,000 bytes.

<!-- <link> -->
<a id="channel-link"></a>

#### `<link>`

The website or web page associated with a podcast. [Official Docs](https://cyber.harvard.edu/rss/rss.html#requiredChannelElements)

````xml
<link>https://www.podstandards.org</link>
````

````xml
<link>https://www.podstandards.org/podcast</link>
````

<!-- <itunes:language> -->
<a id="channel-language"></a>

#### `<language>`

The language that is spoken on the podcast, specified in the [ISO 639](http://www.loc.gov/standards/iso639-2/php/code_list.php) format. [Official Docs](https://cyber.harvard.edu/rss/rss.html#optionalChannelElements)

````xml
<language>en-us</language>
````

<!-- <itunes:category> -->
<a id="channel-itunes-category"></a>

#### `<itunes:category>`

The category that best fits a podcast, selected from the list of [Apple Podcasts categories](https://podcasters.apple.com/support/1691-apple-podcasts-categories). [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:category text="News">
  <itunes:category text="Tech News" />
</itunes:category>
<itunes:category text="Technology" />
````

Multiple category selections are permitted. Categories should be listed in order of priority.

<!-- <itunes:explicit> -->
<a id="channel-itunes-explicit"></a>

#### `<itunes:explicit>`

The parental advisory information for a podcast. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:explicit>false</itunes:explicit>
````

The value can be `true`, indicating the presence of explicit content, or `false`, indicating that a podcast doesn’t contain explicit language or adult content.

<!-- <itunes:image> -->
<a id="channel-itunes-image"></a>

#### `<itunes:image>`

The artwork for the podcast, specified by providing a URL linking to it. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:image href="https://www.podstandards.org/podcast/artwork.jpg" />
````

Verify the web server hosting your image allows HTTP head requests.

Image must be a minimum size of 1400 x 1400 pixels and a maximum size of 3000 x 3000 pixels, in JPEG or PNG format, 72 dpi, with appropriate file extensions (.jpg, .png), and in the RGB colorspace. File type extension must match the actual file type of the image file.

<!--
  ————————————————————————————————
  Recommended `<channel>` elements
  ————————————————————————————————
-->

### Recommended `<channel>` elements

<!-- <podcast:locked> -->
<a id="channel-podcast-locked"></a>

#### `<podcast:locked>`

Tells podcast hosting platforms whether they are allowed to import this feed. [Official Docs](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#locked)

````xml
<podcast:locked>yes</podcast:locked>
````

A value of `yes` means that import attempts should be rejected. A value of `no` means the feed can be imported.

<!-- <podcast:guid> -->
<a id="channel-podcast-guid"></a>

#### `<podcast:guid>`

The globally unique identifier (GUID) for a podcast. The value is a UUIDv5, and generated from the RSS feed URL, with the protocol scheme and trailing slashes stripped off, combined with a unique "podcast" namespace which has a UUID of ead4c236-bf58-58c6-a2c6-a6b28d128cb6. [Official Docs](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#guid)

Example GUID for feed url podnews.net/rss:

````xml
<podcast:guid>9b024349-ccf0-5f69-a609-6b82873eab3c</podcast:guid>
````

A podcast should be assigned a `<podcast:guid>` once in its lifetime, using its current feed URL at the time of assignment as the seed value. That GUID is then meant to follow the podcast from then on, for the duration of its existence, even if the feed URL changes. This means that when a podcast moves from one hosting platform to another, its `<podcast:guid>` should be discovered by the new host and imported into the new platform for inclusion into the feed.

Using this pattern, podcasts can maintain a consistent identity across the open podcasting ecosystem without the need for a central authority.

<!-- <itunes:author> -->
<a id="channel-itunes-author"></a>

#### `<itunes:author>`

The group, person, or people responsible for creating the podcast. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:author>Dallas Times-Herald</itunes:author>
````

<!--
  ———————————————————————————————————————
  Optional supported `<channel>` elements
  ———————————————————————————————————————
-->

### Optional supported `<channel>` elements

The following `<channel>` elements may be used in PSP-complient podcast feeds when relevant.

<!-- `<copyright>` -->
<a id="channel-copyright"></a>

#### `<copyright>`

The copyright details for a podcast. [Official Docs](https://cyber.harvard.edu/rss/rss.html#optionalChannelElements)

````xml
<copyright>The Podcast Standards Project</copyright>
````

Should not include the word "Copyright", the © symbol, and/or a year.

<!-- <podcast:txt> -->
<a id="channel-podcast-txt"></a>

#### `<podcast:txt>`

A free-form text field to present a string in a podcast feed. [Official Docs](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#txt)

````xml
<podcast:txt>naj3eEZaWVVY9a38uhX8FekACyhtqP4JN</podcast:txt>
````

One use case of this is to verify ownership. For example, a show owner may be asked to add a unique `<podcast:txt>` to prove that they control the feed (and therefore the show).

````xml
<podcast:txt purpose="verify">S6lpp-7ZCn8-dZfGc-OoyaG</podcast:txt>
````

The element value is limited to 4,000 characters. This element can optionally be used with a `purpose` attribute, which is also free-form but limited to 128 characters.

<!-- <podcast:funding> -->
<a id="channel-podcast-funding"></a>

### `<podcast:funding>`

> **Note**
> Although `<podcast:funding>` is **optional** for podcast feeds, support for this element is **required** for PSP-certified podcast hosts and players.

This element specifies the donation/funding links for the podcast. The content of the tag is the recommended string to be used with the link. \[[Official Docs](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#funding)\]

````xml
<podcast:funding url="https://www.podstandards.org/support">Support</podcast:funding>
````

<!-- <itunes:type> -->
<a id="channel-itunes-type"></a>

#### `<itunes:type>`

Specifies the podcast as either **episodic** or **serial**. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:type>episodic</itunes:type>
````

````xml
<itunes:type>serial</itunes:type>
````

`episodic` is the default and assumed if this element is not present. This element is required for serial podcasts.

<!-- <itunes:complete> -->
<a id="channel-itunes-complete"></a>

#### `<itunes:complete>`

Specifies that a podcast is complete and will not post any more episodes in the future. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:complete>yes</itunes:complete>
````

The only valid value for this element is `yes`. All other values will be ignored.

### Required `<item>` elements

Each `<item>` in an RSS files represents one podcast episode.

<!-- <title> -->
<a id="item-title"></a>

#### `<title>`

The title for the podcast episode. [Official Docs](https://cyber.harvard.edu/rss/rss.html#ltsourcegtSubelementOfLtitemgt)

````xml
<title>Why podcasts standards are important</title>
````

The title value is a string containing a concise name for your episode. Title values should not include season or episode numbers, as there are specific elements to capture those values.

<!-- <enclosure> -->
<a id="item-enclosure"></a>

#### `<enclosure>`

The audio/video episode content, file size, and file type information. [Official Docs](https://cyber.harvard.edu/rss/rss.html#ltenclosuregtSubelementOfLtitemgt)

````xml
<enclosure length="24986" type="audio/mpeg"
url="https://www.podstandards.org/episode_123.mp3" />
````

The following attributes are **required**: The `length` in bytes, the MIME media `type` (`audio/mpeg`, `audio/m4a`, `video/m4v`, `video/mp4`), and the `URL` of the file. Supported file formats include MP3 (`.mp3`) and MPEG-4 (`.m4a`, `.m4v`, `.mp4`).

<!-- <guid> -->
<a id="item-guid"></a>

#### `<guid>`

The globally unique identifier (GUID) for a podcast episode. [Official Docs](https://cyber.harvard.edu/rss/rss.html#ltguidgtSubelementOfLtitemgt)

````xml
<guid isPermaLink="false">podcast-12345678</guid>
````

Each episode must have a unique GUID that never changes. Values are case-sensitive strings.

<!--
  —————————————————————————————
  Recommended `<item>` elements
  —————————————————————————————
-->

### Recommended `<item>` elements

<!-- <link> -->
<a id="item-link"></a>

#### `<link>`

The URL of a web page associated with the podcast episode. [Official Docs](https://cyber.harvard.edu/rss/rss.html#hrelementsOfLtitemgt)

````xml
<link>https://www.podstandards.org/1983/05/06/post.html</link>
````

Useful when an episode has a corresponding webpage.

#### Item `<pubDate>`

The release date and time of an episode. [Official Docs](https://cyber.harvard.edu/rss/rss.html#ltpubdategtSubelementOfLtitemgt)

````xml
<pubDate>Fri, 26 Feb 2021 00:00:00 -0500</pubDate>
````

Values must use the [RFC 2822](http://www.faqs.org/rfcs/rfc2822.html) specifications.

<!-- <description> -->
<a id="item-description"></a>

#### Item `<description>`

The description of the podcast episode. [Official Docs](https://cyber.harvard.edu/rss/rss.html#hrelementsOfLtitemgt)

````xml
<description>Sample episode description.</description>
````

````xml
<description><![CDATA[<p>Sample episode description.</p>]]></description>
````

The maximum amount of text allowed for this tag is 4000 bytes. Some HTML is permitted (`<p>`, `<ol>`, `<ul>`, `<li>`, `<a>`, `<b>`, `<i>`, `<strong>`, `<em>`) if wrapped in the `<CDATA>` tag.

#### Item `<itunes:duration>`

The duration of a podcast episode in seconds. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:duration>1801</itunes:duration>
````

<!-- <itunes:image> -->
<a id="item-itunes-image"></a>

#### `<itunes:image>`

The episode-specific artwork. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:image href="https://www.podstandards.org/podcast/episode/1/artwork.jpg" />
````

Verify the web server hosting your image allows HTTP head requests.

Image must be a minimum size of 1400 x 1400 pixels and a maximum size of 3000 x 3000 pixels, in JPEG or PNG format, 72 dpi, with appropriate file extensions (.jpg, .png), and in the sRGB colorspace. File type extension must match the actual file type of the image file.

<!-- <itunes:explicit> -->
<a id="item-itunes-explicit"></a>

#### `<itunes:explicit>`

The parental advisory information for a podcast episode. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:explicit>true</itunes:explicit>
````

````xml
<itunes:explicit>false</itunes:explicit>
````

The value can be `true`, indicating the presence of explicit content, or `false`, indicating that a podcast doesn’t contain explicit language or adult content.

<!-- <podcast:transcript> -->
<a id="item-podcast-transcript"></a>

#### `<podcast:transcript>`

A link to a transcript or closed captions file. Multiple tags can be present for multiple formats. [Official Docs](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#transcript)

````xml
<podcast:transcript url="https://www.podstandards.org/episode1/transcript.html" type="text/html" />
<podcast:transcript url="https://www.podstandards.org/episode1/transcript.srt" type="application/x-subrip" rel="captions" />
<podcast:transcript url="https://www.podstandards.org/episode1/transcript.vtt" type="text/vtt" />
<podcast:transcript url="https://www.podstandards.org/episode1/transcript.json" type="application/json" language="es" rel="captions" />
````

Must contain two attributes: The `URL` of the podcast transcript, and the `type` (`text/plain`, [`text/html`](https://github.com/Podcastindex-org/podcast-namespace/blob/main/transcripts/transcripts.md#html), [`text/vtt`](https://github.com/Podcastindex-org/podcast-namespace/blob/main/transcripts/transcripts.md#webvtt), [`application/json`](https://github.com/Podcastindex-org/podcast-namespace/blob/main/transcripts/transcripts.md#json), [`application/x-subrip`](https://github.com/Podcastindex-org/podcast-namespace/blob/main/transcripts/transcripts.md#srt)).

<!--
  ————————————————————————————————————
  Optional supported `<item>` elements
  ————————————————————————————————————
-->

### Optional supported `<item>` elements

The following `<item>` elements may be used in PSP-complient podcast feeds when relevant.

<!-- <itunes:episode> -->
<a id="item-itunes-episode"></a>

#### `<itunes:episode>`

The chronological number that is associated with a podcast episode. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:episode>46</itunes:episode>
````

Must be a non-zero integer. This is required for serial podcasts.

<!-- <itunes:season> -->
<a id="item-itunes-season"></a>

#### `<itunes:season>`

The chronological number associated with a podcast episode's season. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:season>2</itunes:season>
````

Must be a non-zero integer.

<!-- <itunes:episodeType> -->
<a id="item-itunes-episode-type"></a>

#### `<itunes:episodeType>`

Defines the type of content for a specific podcast episode. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:episodeType>full</itunes:episodeType>
````

- `full` (assumed if absent) – A complete podcast episode
- `trailer` – A short promotional or preview episode for a podcast
- `bonus` – Additional content that is unlike a typical episode (e.g., behind-the-scenes or a promotional episode for another podcast)

<!-- <itunes:block> -->
<a id="item-itunes-block"></a>

#### `<itunes:block>`

Prevents a specific episode from appearing in podcast listening applications. [Official Docs](https://help.apple.com/itc/podcasts_connect/#/itcb54353390)

````xml
<itunes:block>yes</itunes:block>
````

The only valid value for this element is `yes`. All other values will be ignored.

\###

### Recent changes

- Added a bunch of context at the top of the document
- Conformed to `markdownlint` style suggestions whenever possible
- Added explicit internal links because Markdown doesn't like duplicate heading names
- Added comments that make it easier to navigate source
