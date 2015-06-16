gogrepo
-------
Python-based tool for downloading all your GOG.com game and bonus collections to your local computer for full offline enjoyment.

It is a clean standalone python script that can be run from anywhere. It requires a typical Python 2.7 installation and html5lib.

By default, game folders are saved in the same location that the script is run in. You can also specify another
directory. Run gogrepo.py -h to see help or read more below. Each game has its own directories with all game/bonus files saved within.

License: GPLv3+

Features
--------
* Ability to choose which games to download based on combinations of OS (win, linux, osx) and language (en, fr, etc...)
* Saves a !info.txt in each game folder with information about each game/bonus item.
* creates a !serial.txt if the game has a special serial/cdkey (I know, not 100% DRM-free, is it?). Sometimes coupon codes are hidden here!
* Verify your downloaded collection with full MD5, zip integrity, and expected file size checking.
* Auto retrying of failed fetch/downloads. Sometime GOG servers report temporary errors.
* Ability to import your already existing local collection.
* Easy to throw into a daily cronjob to get all the latest updates and newly added content!
* Clear logging prints showing update/download progress and HTTP errors. Easy to pipe or tee to create a log file.


Quick Start -- Typical Use Case
----------------

* Login to GOG and save your login cookie for later commands. Your login/pass can be specified or be prompted. You generally only need to do this once to create a valid gog-cookies.dat

  ``gogrepo.py login``

* Fetch all game and bonus information from GOG for items that you own and save into a local manifest file. Run this whenever you want to discover newly added games or game updates.

  ``gogrepo.py update -os windows linux mac -lang English``

* Download the games and bonus files for the OS and languages you want for all items known from the saved manifest file.

  ``gogrepo.py download``

* Verify and report integrity of all downloaded files. Does MD5, zip integrity, and expected filesize verification. This makes sure your game files can actually be read back and are healthy.

  ``gogrepo.py verify``


Commands
--------

``gogrepo.py login`` Authenticate with GOG and save the cookie locally in gog-cookies.dat file. This is needed to do
update or download command. Run this once first before doing update and download.

    login [-h] [username] [password]
    -h, --help  show this help message and exit
    username    GOG username/email
    password    GOG password

--

``gogrepo.py update`` Fetch game data and information from GOG.com for the specified operating systems and languages. This collects file game titles, download links, serial numbers, MD5/filesize data and saves the data locally in a manifest file. Manifest is saved in a gog-manifest.dat file

    update [-h] [-os [OS [OS ...]]] [-lang [LANG [LANG ...]]]
    -h, --help            show this help message and exit
    -os [OS [OS ...]]     operating system(s) (ex. win linux osx)
    -lang [LANG [LANG ...]]  game language(s) (ex. en fr)

--

``gogrepo.py download`` Use the saved manifest file from an update command, and download all known game items and bonus files.

    download [-h] [-dryrun] [-skipextras] [-wait WAIT] [savedir]
    savedir      directory to save downloads to
    -h, --help   show this help message and exit
    -dryrun      display, but skip downloading of any files
    -skipextras  skip downloading of any GOG extra files
    -wait WAIT   wait this long in hours before starting

--

``gogrepo.py verify`` Check all your game files against the save manifest data, and verify MD5, zip integrity, and
expected file size. Any missing or corrupt files will be reported.

    verify [-h] [-skipmd5] [-skipsize] [-skipzip] [-delete] [gamedir]
    gamedir     directory containing games to verify
    -h, --help  show this help message and exit
    -skipmd5    do not perform MD5 check
    -skipsize   do not perform size check
    -skipzip    do not perform zip integrity check
    -delete     delete any files which fail integrity test

--

``gogrepo.py import`` Search an already existing GOG collection for game item/files, and import them to your
new GOG folder with clean game directory names and file names as GOG has them named on their servers.

    import [-h] src_dir dest_dir
    src_dir     source directory to import games from
    dest_dir    directory to copy and name imported files to
    -h, --help  show this help message and exit

--

``gogrepo.py backup`` Make copies of all known files in manifest file from a source directory to a backup destination directory. Useful for cleaning out older files from your GOG collection.

    backup [-h] src_dir dest_dir
    src_dir     source directory containing gog items
    dest_dir    destination directory to backup files to
    -h, --help  show this help message and exit


Requirements
------------
* Python 2.7 (Python 3 support coming soon)
* Latest version of html5lib (https://github.com/html5lib/html5lib-python)


TODO
----
* add GOG movie support
* add support for incremental manifest updating (ie. only fetch newly added games) rather than fetching entire collection information
* ability to customize/remap default game directory name
* command to remove old or unexpected files/installers/patches without having to run a backup first
* feel free to contact me with ideas or feature requests


About
-----
I am a big fan of GOG and what they do.  They have brought back purchasable classic games and enforce a DRM-free policy
that makes me feel like I own the content that I've purchased. I like the idea of being able to download, locally save,
and choose and play anything I want whenever I want without an online dependency. I thought it would be a nice home
project outside of my fulltime job to make a script that downloaded and kept my collection up to date locally.

Special thanks to evenpowers and his https://github.com/evanpowers/gog-backup github project. I found it while googling
to see if other such projects already existed (preferably in one of my favorite languages, Python). It doesn't work anymore
and hasn't been updated since Feb 2012, but it has some nice salvageable ideas and code. I consider gogrepo
to be somewhat of a fork and spiritual successor of gog-backup with some features added and removed.
If there is any feature you miss from gog-backup, let me know and I will probably add it.