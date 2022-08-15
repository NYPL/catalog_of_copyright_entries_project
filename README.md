
# Catalog of Copyright Entries Project
NYPL Project to transcribe and parse pages from the US Catalog of Copyright Entries

The New York Public Library (NYPL) is embarking on a pilot project to extract the data from a publication known as the Catalog of Copyright Entries, published annually by the United States Copyright Office. The volumes have already been digitized and are [freely available through the Internet Archive](https://archive.org/details/copyrightrecords); our project aims to extract and parse the data contained in the records in order to create a searchable database that will aid copyright research.

For more on the project, see ["Unlocking the Record of American Creativity—with Your Help"](https://www.nypl.org/blog/2018/03/30/unlocking-record-american-creativity)

For more on the catalog, see the following: 

- [UPenn Online Books Page for Copyright Catalog](http://onlinebooks.library.upenn.edu/cce/)
- [Wikipedia article](https://en.wikipedia.org/wiki/Copyright_Catalog)
- [Catalog Volumes on Internet Archive](http://archive.org/details/copyrightrecords/)

Although all of the data extracted to date from the CCEs by the NYPL team is available on this GitHub repo, NYPL has added records from prior CCE projects and made it available in [this unofficial, experimental search interface](https://cce-search.nypl.org/).

## Data Structure and Contents

All data files are in the [./xml](./xml/) directory, organized by year. The XML files conform to the [project DTD](CopyrightEntries.dtd), and each directory has an `alto` subdirectory with [ALTO](https://altoxml.github.io/) format files for the original OCR.

See [TOC.md](xml/TOC.md) for details on the volumes transcribed so far.

### CopyrightEntries.dtd

The main components of an XML files, within the root `<copyrightEntries>` element are a mandatory `<header>` followed by any order of `<copyrightEntry>`, `<entryGroup>`, `<crossRef>` and `<pgNum>` elements.

There are tags for identifyings authors, titles, publishers, and claimants, as well as the various dates and id numbers that an entry can contain. Many entries have attributes for recording normalized versions of dates and numbers or for identifying where corrections have been made.

 See the [Guide](./guide.md) for specifics of formatting entries.

### Anatomy of a Registration

The format of entries in the _Catalog_ varies widely over time but they essentialy contain simple bibliographic information and a registration date and id number.

    ADAMS, JAMES DONALD.

      Literary frontiers.  New
        York, Duell, Slone and
        Pearce.  175 p. © J. Donald 
		Adams; 6Jun51; A56505.

This is converted to XML:

    <copyrightEntry 
         id="1D4D33CD-6E97-1014-8315-97D5E63C7536"
         regnum="A56505">
      <author>
        <authorName>ADAMS, JAMES DONALD</authorName>.
      </author> 
      <title>Literary frontiers.</title>
      <publisher>
        <pubPlace>New York</pubPlace>, 
        <pubName>Duell, Sloan and Pearce.</pubName> 
      </publisher>
      <desc>175 p.</desc> 
      &#xA9; <claimant>J. Donald Adams</claimant>;
      <regDate date="1951-06-06">6Jun51</regDate>; 
      <regNum>A56505</regNum>.
    </copyrightEntry>
    
Our top priority is to correctly tag the registration numbers and dates since these are required to match registrations to renewals. Next in priority are the authors and titles although for practical purposes a full-text search is probably adequate to find an entry.

### Identifiers

Every registration should have a registration number, such as `A56505`, but these are not unique. Numbering was restarted in "Third Series" (1947–) so there is quite a bit of overlap between this and the "New Series." For example, the example entry above shares a registration number with another book _Barton Warren Stone, pathfinder of Christian union; a story of his life and times_ registered in 1932. Because of this a registration number _and_ date is always required to distinguish `A56505/1951-06-06` from `A56505/1932-10-12`

In addition every `<copyrightEntry`> and `<crossRef>` is assigned a UUID so that it can be uniquely identified, even if the registration number or date is changed (for instance, to correct a typo).

### Renewals

These volumes were chosen to transcribe first because they come from the period when a book may in copyright if its first 28-year copyright term was renewed, while it is otherwise public domain. Renewal data is available from the Stanford [Copyright Renewals Database](https://exhibits.stanford.edu/copyrightrenewals) and from an [NYPL version](https://github.com/NYPL/cce-renewals/) of essentially the same sources. The NYPL version is better formatted for matching renewal entries with the registrations in these XML files.

By combining the two datasets we can determine how many books were registered for copyright in every year between 1923 and 1963, as well as how many were renewed:

![Chart showing the number of books registered and renewed each year, 1923-1963](cce-renewal-rate.png)

For this period we have about 642,000 books registered for copyright. Of these about 162,000 or 25% had their copyrights renewed. So, the copyright has expired on 75% of the books published during these years, about 480,000, and they are now in the Public Domain.


## User Stories for Search Interface
Although we do not have an official search interface for this data yet (the [unofficial, experimental interface is here](https://cce-search.nypl.org/)), we gathered a group of experts in Copyright Office records to discuss user needs and requirements for a search interface system. That group prioritized a list of [user stories](https://github.com/NYPL/catalog_of_copyright_entries_project/wiki/User-Stories) that could be used to start to develop such a search interface system.

## CCE Information and Organization Sheets
The project team's goal is to complete the transcription and parsing of all of the CCEs. To keep us organized, we've created a number of spreadsheets in Google Sheets. While all of these sheets are works-in-progress, you may find the data useful. We will create a sheet for each major type of work or category within the CCE. Each sheet should include information about the particular CCE volumes relevant to that category, including direct links to each CCE volume, the page numbers that bound each category, the number of expected records based on either the counts included by the Copyright Office in the front matter for each volume or on the record count (depending on year), and other relevant data. For many years, data was published in cycles shorter than one year, creating multiple sections (in CCE lingo, "Numbers") for a single year. The data for each section is recorded in individual tabs for that year. As more sheets are built, sections will be added below.

##### Overview of CCE Category Changes Over Time
An overview of the organization of the CCEs from 1906 to 1977 is available [here](https://docs.google.com/spreadsheets/d/1EoPt7MggV7ZPgJUljD1wC2h2cIy1aiktBHzU4l6OEuQ/edit?usp=sharing). We've also built a [directory of links](https://docs.google.com/spreadsheets/d/1IwX2bU2UzPP6sW0Nz15RbW2YxczRqQB-w4a-NqEXswY/edit?usp=sharing) to the CCEs as they are presented in Internet Archive. Eventually, we will fill in the data for pre-1923 CCEs for both of these sheets.

##### Part 1: Books
Although the Books part designation changed over time (Part 1 Group 1 in 1923, Part 1A starting in 1947, and Part 1 starting in 1953) we've combined the data about books into [this sheet](https://docs.google.com/spreadsheets/d/14u_0gfhBnDvpuBTT8gqp1Hv-lSmK_DYKDXBfgUaVP2k/edit?usp=sharing).

##### Part 1B: Pamphlets, Leaflets, and More
From 1923-mid-1953, the CCEs grouped a number of different kinds of works into a sub-part of the Books category. Depending on year, this group included pamphlets, leaflets, contributions to newspapers or periodicals, lectures, sermons, addresses for oral delivery, dramatic compositions, maps, and motion pictures. A sheet with information about this sub-category for books is [available here](https://docs.google.com/spreadsheets/d/1iXdOCV2H6pmNDG1CHOnkvJTRSOCb8Uq2TjU3UGEhRjY/edit?usp=sharing).

##### Part 2: Periodicals
Periodicals have enjoyed their own category within the CCE. A sheet tracking information about this category can be found [here](https://docs.google.com/spreadsheets/d/1UrDf4zFfa7mbG0mw2gAWWLhQMqEb1sZZjtmgaxMnNaE/edit?usp=sharing).

##### Part 5: Musical Compositions
Data about musical composition registrations and renewals can be found [here](https://docs.google.com/spreadsheets/d/1YzOtE1ipOiIcWlTMK7FXn7NAERQxAO7towlbU-rHrPc/edit?usp=sharing).



## Status of Project

As of 8/15/22, the registrations for the "Books" category from 1923-1977 has been completed and data uploaded to this repository.

As time permits, the team is recording information about the relevant CCE volumes for each category of work.

The project team continues to pursue funding opportunities to tackle the transcription and parsing of the remaining CCEs.

***
**Press Inquiries**: Please contact [Greg Cram](mailto:gregcram@nypl.org) or [Sean Redmond](mailto:seanredmond@nypl.org)
