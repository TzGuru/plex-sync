# plex-sync

A simple command-line utility to synchronize watched / seen status between different [Plex Media Servers](https://plex.tv).

[![npm](https://img.shields.io/npm/v/plex-sync.svg?maxAge=2592000)](https://www.npmjs.com/package/plex-sync)
[![Travis](https://img.shields.io/travis/jacobwgillespie/plex-sync.svg?maxAge=2592000)](https://travis-ci.org/jacobwgillespie/plex-sync)
[![Dependencies](https://david-dm.org/jacobwgillespie/plex-sync.svg)](https://david-dm.org/jacobwgillespie/plex-sync)
![MIT license](https://img.shields.io/badge/license-MIT-blue.svg?maxAge=2592000)

## Features

* Syncs watch status between different Plex servers.

## Requirements

* NodeJS 4+

## Installation

`plex-sync` is installed via NPM:

```shell
$ npm install -g plex-sync
```

## Usage

There are several available configuration environment variables:

Variable | Description
-------- | -----------
`PLEX_TOKEN` | **Required.**  The API token used to access your Plex server.  To locate this token, visit a media item page (like for a movie or TV episode), click on the **Info** link in the item menu (the **...**), then choose "View XML" at the bottom of the info dialog.  Look in the URL bar at the very end of the URL for something like `X-Plex-Token=xxxYOUR_TOKEN_HERExxx`.  That is your authentication token.
`DRY_RUN` | Set this environment variable to make `plex-sync` print out what it was planning to do rather than actually perform the synchronization.
`MATCH_TYPE` | Can be either `fuzzy` (default) or `precise`.  When the matching is fuzzy, the script will match items by their year and title.  When the matching is precise, the script matches items by their internal Plex GUID, which is usually the IMDb or TMDb ID.  This requires an individual API request to be performed for each item (each movie, each TV episode, etc.) and thus is very slow and can potentially overwhelm and crash the Plex server.  Use at your own risk.
`RATE_LIMIT` | Default `5`.  If the `MATCH_TYPE` is set to `precise`, this is the maximum number of concurrent API requests `plex-sync` will make to your server to fetch GUIDs.  Use this to (potentially) alleviate performance issues with precise matching.

First, find the IDs for the libraries on each server you would like to sync.  These IDs can be found at the end of the URL when viewing the library in your browser (like `.../section/ID`).

Next, use the CLI as follows:

```shell
$ plex-sync IP[:PORT]/SECTION IP[:PORT]/SECTION [IP[:PORT]/SECTION...]
```

### Examples

Sync watched status between two servers, using the default port (`32400`), using library ID `1` for the first server and library `3` for the second:

```shell
$ plex-sync 10.0.1.5/1 10.0.1.10/3
```

Sync three servers, with different ports:

```shell
$ plex-sync 10.0.1.5:32401/1 10.0.1.5:32402/1 10.0.1.10/3
```

Dry run, to see what the script will do:

```shell
$ DRY_RUN=1 plex-sync 10.0.1.5/1 10.0.1.5/1
```

Precise matching (slow and may crash the Plex server):

```shell
$ MATCH_TYPE=precise plex-sync 10.0.1.5/1 10.0.1.5/1
```

For more complex strategies, like syncing between multiple different library mappings, just run the tool multiple times.  If you need to run the synchronization on a schedule, use another scheduling tool like cron.  These more advanced features may be added in the future, but currently `plex-sync` is very simple.

## Contributing

Contributions are welcome.  Open a pull request or issue to contribute.

## License

MIT license.  See `LICENSE` for more information.
