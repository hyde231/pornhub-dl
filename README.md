# Pornhub-dl

Download all videos of your favorite pornhub models, playlists and channels and update all your stuff with a simple command.

## Setup
1. You will need `poetry` for dependency management and venv creation: `poetry install`
2. The default config will create a new postgres database `pornhub` when first running the program.
Make sure your user has proper rights.
If you want to change this, you need to adjust the sqluri in `db.py`.

3. If you want to be up to date with migrations, set the alembic head to your current version.
You can use this by running `poetry run alembic stamp head`.


## Migrating

Just execute `poetry run alembic upgrade head`.

## Usage
The project is used by invocing `pornhub-dl`. In combination with poetry this looks like this: `poetry run python pornhub-dl`  

There is a help for all commands, but here are some examples anyway:

`pornhub-dl user [some_model]` Follow this user/model/pornstar and download all videos.  
`pornhub-dl playlist [playlist_id]` Follow this playlist and download all videos.  
`pornhub-dl channel [channel_id]` Follow this channel and download all videos.  
`pornhub-dl video [vkey]` Download a single video by viewkey e.g. `ph56e961d32ce26`  
`pornhub-dl update` Get the newest videos of all your followed models, playlists and channels.  
`pornhub-dl reset` Reschedule all files for download. Useful if you got your hands on a premium account ;)   
`pornhub-dl remove` Remove a user, if it no longer exists  
`pornhub-dl rename` Rename a user, if they chainged their user key  


## How does it work?

All videos are downloaded into a folder with the name of the respective user.
Pornhub-dl uses youtube-dl as a download backend, but implements own functionality, such as custom user meta-data extraction, user/channel/playlist following, as well as video list extraction and retry logic for rate limiting.

Pornhub-dl crawls all video urls of all followed entities and remembers the ids of downloaded videos.
If you update your database, new videos will be downloaded, while old ones are simply skipped without invoking youtube-dl.

## Configuration
When starting the downloader for the first time, a configuration file is created at `~/.config/pornhub_dl.toml`


- `location`: Video download location. Default: `~/pornhub/`
- `sql_uri`: Your sql location. Default: `'postgres://localhost/pollbot'`


## Premium

To enable premium, copy your Pornhub cookies and paste them to `./cookie_file`.
The cookies file must be formatted after the `Netscape cookie file format`.
The cookie file can be, for instance, created with his tool:

https://addons.mozilla.org/en-US/firefox/addon/cookies-txt/

On top of that, create a second file `http_cookie_file`
Simply copy the `Cookie` header of any logged in pornhub premium domain request.
Those can be extracted using your browser's network debugging tool.


**Disclaimer:**

This project is not associated in any way with the operators of the official pornhub.com
It's just a small and fun side-project in reaction to the possibly imminent censorship in the EU and free global premium.
