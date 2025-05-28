### Retarrd Disclaimer

\[To my knowledge\] There isn't a tool to keep an up-to-date copy of the best stuff (not IsBest) from SeaDex so I decided to make something for *personal use*. This repo is just to track my progress and think out loud.

I have a working spaghetti code PoC but its too Retarrded. Gonna rewrite with Docker + AniList integration and publish a 1.0 hopefully in a month or so

### No private trackers

Filtering out private servers b/c I'm not on any of them so I'd appreciate invites <3
I have around 120TB of drives with a potential 500TB if I fill in all the slot.

### Thinking out loud

* Pull data from SeaDex
    * [Ravencentric/seadex](https://github.com/Ravencentric/seadex) is very cool but I already did some manual queries so I won't use it (I'm too lazy to learn how to use it)
* Store SeaDex Data in a DB
  * Main identifier is AniList ID
  * Parse expand:trs key and combine torrents by release or some other IDs that makes sense (each link is a separate trs for some reason lmao)
  * Filter out private trackers b/c I'm not on any of them :(
everything i.e. only keep A & C
* Pull AniList.co data   
  * OPTIONAL: Map AniList.co to AniDB, TVDB, or TMDB (or integrate something else that does)
* Pick best release
  * SeaDex Best + Dual Audio > SeaDex Best > SeaDex Alt + Dual Audio > SeaDex Alt
  * Tie breaks decided by release with higher index (higher index is = at top on SeaDex = better)
* Pull .torrent file from nyaa and store it alongside each trs entry
  * OPTIONAL: Add animetosho and the othershit they use sometimes
* Best release info (release group, infohash, .torrent file, etc.) -> "BestRelease" DB entry for that show (i.e. BestRelease:ReleaseGroup, BestRelease:InfoHashes, etc.)
* qBit Integration
    * Create categories as needed
      * ALID = category
      * Category save path based on AniList and seadex data:  `<seadex>/<movie,tv,short,special,ona,ova>/<alt,best>/<best,alt>/<complete,incomplete>/<show name> (<year>) {alid if anything uses it or aniDB id for Shoko}`
    * Set content layout to don't create subdirectory
    * Set tags/categories as needed
    * On Complete Command set to ping server with infohash of the torrent
      * On server: received info hash -> "OnDisk:infohashes" DB entry for that show (add other data from OnDisk:BestRelease)
      * if "OnDisk:InfoHashes" == "BestRelease:InfoHashes": compute hashes for all files on disk and store to OnDisk:files (keep track of shit and integrity even if torrent is removed from qbittorrent). 
* Downloaded show's data (release group, infohash, all files hash?)-> "On Disk" DB entry for that show
* NEVER: WebUI

Here's the directory structure I'm going for:

```
tv
movies
books
...
seadex
├── movie
│   ├── alt
│   │   ├── complete
│   │   └── incomplete
│   └── best
│       ├── complete
│       └── incomplete
├── ona
│   ├── alt
│   │   ├── complete
│   │   └── incomplete
│   └── best
│       ├── complete
│       └── incomplete
├── ova
│   ├── alt
│   │   ├── complete
│   │   └── incomplete
│   └── best
│       ├── complete
│       └── incomplete
├── short
│   ├── alt
│   │   ├── complete
│   │   └── incomplete
│   └── best
│       ├── complete
│       └── incomplete
├── special
│   ├── alt
│   │   ├── complete
│   │   └── incomplete
│   └── best
│       ├── complete
│       └── incomplete
└── tv
    ├── alt
    │   ├── complete
    │   └── incomplete
    └── best
        ├── complete
        └── incomplete
```

### FAQ

**Why Retarrd?**
> Because its funny. The other idea was Gaydarr but Whisparr already handles gay stuff
