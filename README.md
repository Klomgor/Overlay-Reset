# Plex Meta Manager Overlay Reset

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/meisnate12/PMM-Overlay-Reset?style=plastic)](https://github.com/meisnate12/PMM-Overlay-Reset/releases)
[![Docker Image Version (latest semver)](https://img.shields.io/docker/v/meisnate12/pmm-overlay-reset?label=docker&sort=semver&style=plastic)](https://hub.docker.com/r/meisnate12/pmm-overlay-reset)
[![Docker Pulls](https://img.shields.io/docker/pulls/meisnate12/pmm-overlay-reset?style=plastic)](https://hub.docker.com/r/meisnate12/pmm-overlay-reset)
[![Develop GitHub commits since latest stable release (by SemVer)](https://img.shields.io/github/commits-since/meisnate12/PMM-Overlay-Reset/latest/develop?label=Commits%20in%20Develop&style=plastic)](https://github.com/meisnate12/PMM-Overlay-Reset/tree/develop)

[![Discord](https://img.shields.io/discord/822460010649878528?color=%2300bc8c&label=Discord&style=plastic)](https://discord.gg/NfH6mGFuAB)
[![Reddit](https://img.shields.io/reddit/subreddit-subscribers/PlexMetaManager?color=%2300bc8c&label=r%2FPlexMetaManager&style=plastic)](https://www.reddit.com/r/PlexMetaManager/)
[![Wiki](https://img.shields.io/readthedocs/plex-meta-manager?color=%2300bc8c&style=plastic)](https://metamanager.wiki/en/latest/home/scripts/overlay-reset.html)
[![GitHub Sponsors](https://img.shields.io/github/sponsors/meisnate12?color=%238a2be2&style=plastic)](https://github.com/sponsors/meisnate12)
[![Sponsor or Donate](https://img.shields.io/badge/-Sponsor%2FDonate-blueviolet?style=plastic)](https://github.com/sponsors/meisnate12)

Plex Meta Manager Overlay Reset is an open source Python 3 project that has been created to Remove all Overlays placed on a Plex Library.

## Installing PMM Overlay Reset

Generally, PMM Overlay Reset can be installed in one of two ways:

1. Running on a system as a Python script [we will refer to this as a "local" install]
2. Running as a Docker container

GENERALLY SPEAKING, running as a Docker container is simpler, as you won't have to be concerned about installing Python, or support libraries, or any possible system conflicts generated by those actions.

For this reason, it's generally recommended that you install via Docker rather than directly on the host.

If you have some specific reason to avoid Docker, or you prefer running it as a Python script for some particular reason, then this general recommendation is not aimed at you.  It's aimed at someone who doesn't have an existing compelling reason to choose one over the other.

### Install Walkthroughs

There are no detailed walkthroughs specifically for PMM Overlay Reset but the process is extremely similar to how you would do it with [Plex Meta Manager](https://metamanager.wiki/en/latest/home/installation.html#install-walkthroughs).

### Local Install Overview

PMM Overlay Reset requires Python 3.11 or later. Later versions may function but are untested.

These are high-level steps which assume the user has knowledge of python and pip, and the general ability to troubleshoot issues. 

1. Clone or [download and unzip](https://github.com/meisnate12/PMM-Overlay-Reset/archive/refs/heads/master.zip) the repo.

```shell
git clone https://github.com/meisnate12/PMM-Overlay-Reset
```
2. Install dependencies:

```shell
pip install -r requirements.txt
```

3. If the above command fails, run the following command:

```shell
pip install -r requirements.txt --ignore-installed
```

At this point PMM-Overlay-Reset has been installed, and you can verify installation by running:

```shell
python pmm_overlay_reset.py
```

### Docker Install Overview

#### Docker Run:

```shell
docker run -v <PATH_TO_CONFIG>:/config:rw meisnate12/pmm-overlay-reset
```
* The `-v <PATH_TO_CONFIG>:/config:rw` flag mounts the location you choose as a persistent volume to store your files.
  * Change `<PATH_TO_CONFIG>` to a folder where your .env and other files are.
  * If your directory has spaces (such as "My Documents"), place quotation marks around your directory pathing as shown here: `-v "<PATH_TO_CONFIG>:/config:rw"`

Example Docker Run command:

These docs are assuming you have a basic understanding of Docker concepts.  One place to get familiar with Docker would be the [official tutorial](https://www.docker.com/101-tutorial/).

```shell
docker run -v "X:\Media\PMM Overlay Reset\config:/config:rw" meisnate12/pmm-overlay-reset
```

#### Docker Compose:

Example Docker Compose file:
```yaml
version: "2.1"
services:
  plex-meta-manager:
    image: meisnate12/pmm-overlay-reset
    container_name: pmm-overlay-reset
    environment:
      - TZ=TIMEZONE #optional
    volumes:
      - /path/to/config:/config
    restart: unless-stopped
```

#### Dockerfile

A `Dockerfile` is included within the GitHub repository for those who require it, although this is only suggested for those with knowledge of dockerfiles. The official PMM Overlay Reset build is available on the [Dockerhub Website](https://hub.docker.com/r/meisnate12/pmm-overlay-reset).

## Options

Each option can be applied in three ways:

1. Use the Shell Command when launching.
2. Setting the Environment Variable.
3. Adding the Environment Variables to `config/.env` 

| Option                  | Description                                                                                                                                                                                                                                                                         | Required |
|:------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------:|
| Plex URl                | Plex URL of the Server you want to connect to.<br>**Shell Command:** `-u` or `--url "http://192.168.1.12:32400"`<br>**Environment Variable:** `PLEX_URL=http://192.168.1.12:32400`                                                                                                  | &#9989;  |
| Plex Token              | Plex Token of the Server you want to connect to.<br>**Shell Command:** `-t` or `--token 123456789`<br>**Environment Variable:** `PLEX_TOKEN=123456789`                                                                                                                              | &#9989;  |
| Plex Library            | Plex Library Name you want to reset.<br>**Shell Command:** `-l` or `--library Movies`<br>**Environment Variable:** `PLEX_LIBRARY=Movies`                                                                                                                                            | &#9989;  |
| PMM Asset Folder        | Plex Meta Manager Asset Folder to Scan for restoring posters.<br>**Shell Command:** `-a` or `--asset "C:\Plex Meta Manager\config\assets"`<br>**Environment Variable:** `PMM_ASSET=C:\Plex Meta Manager\config\assets`                                                              | &#10060; |
| PMM Original Folder     | Plex Meta Manager Original Folder to Scan for restoring posters.<br>**Shell Command:** `-o` or `--original "C:\Plex Meta Manager\config\overlays\Movies Original Posters"`<br>**Environment Variable:** `PMM_ORIGINAL=C:\Plex Meta Manager\config\overlays\Movies Original Posters` | &#10060; |
| TMDb V3 API Key         | TMDb V3 API Key for restoring posters from TMDb.<br>**Shell Command:** `-ta` or `--tmdbapi 123456789123456789`<br>**Environment Variable:** `TMDBAPI=123456789123456789`                                                                                                            | &#10060; |
| Start From              | Plex Item Title to Start restoring posters from.<br>**Shell Command:** `-st` or `--start "Mad Max"`<br>**Environment Variable:** `START=Mad Max`                                                                                                                                    | &#10060; |
| Items                   | Restore specific Plex Items by Title. Can use a bar-separated (<code>&#124;</code>) list.<br>**Shell Command:** `-it` or <code>--items "Mad Max&#124;Mad Max 2"</code><br>**Environment Variable:** <code>ITEMS=Mad Max&#124;Mad Max 2</code>                                       | &#10060; |
| Timeout                 | Timeout can be any number greater then 0. **Default:** `600`<br>**Shell Command:** `-ti` or `--timeout 1000`<br>**Environment Variable:** `TIMEOUT=1000`                                                                                                                            | &#10060; |
| Dry Run                 | Run as a Dry Run without making changes in Plex.<br>**Shell Command:** `-d` or `--dry`<br>**Environment Variable:** `DRY_RUN=True`                                                                                                                                                  | &#10060; |
| Flat Assets             | PMM Asset Folder uses [Flat Assets Image Paths](https://metamanager.wiki/en/latest/home/guides/assets.html#asset-naming).<br>**Shell Command:** `-f` or `--flat`<br>**Environment Variable:** `PMM_FLAT=True`                                                                       | &#10060; |
| Reset Season Posters    | Restore Season posters during run.<br>**Shell Command:** `-s` or `--season`<br>**Environment Variable:** `SEASON=True`                                                                                                                                                              | &#10060; |
| Reset Episode Posters   | Restore Episode posters during run.<br>**Shell Command:** `-e` or `--episode`<br>**Environment Variable:** `EPISODE=True`                                                                                                                                                           | &#10060; |
| Ignore Automatic Resume | Ignores the automatic resume.<br>**Shell Command:** `-ir` or `--ignore-resume`<br>**Environment Variable:** `IGNORE_RESUME=True`                                                                                                                                                    | &#10060; |
| Trace Logs              | Run with extra trace logs.<br>**Shell Command:** `-tr` or `--trace`<br>**Environment Variable:** `TRACE=True`                                                                                                                                                                       | &#10060; |
| Log Requests            | Run with every request logged.<br>**Shell Command:** `-lr` or `--log-requests`<br>**Environment Variable:** `LOG_REQUESTS=True`                                                                                                                                                     | &#10060; |


### Example .env File
```
PLEX_URL=http://192.168.1.12:32400
PLEX_TOKEN=123456789
PLEX_LIBRARY=Movies
PMM_ASSET=C:\Plex Meta Manager\config\assets
PMM_ORIGINAL=C:\Plex Meta Manager\config\overlays\Movies Original Posters
TMDBAPI=123456789123456789
START=
ITEMS=
TIMEOUT=600
DRY_RUN=True
PMM_FLAT=False
SEASON=True
EPISODE=True
IGNORE_RESUME=False
TRACE=False
LOG_REQUESTS=False
```