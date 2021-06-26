# YouTube Archive
This is a repo dedicated to managing the archival of unlisted YouTube videos uploaded before 01/01/2017. 
YouTube will be making them all private in approximately a month.

**We are currently focusing only on unlisted videos made before 01/01/2017.**

## How to Help?
You don't necessarily need to download to help the effort. Submitting lists of IDs of unlisted videos is very helpful.
Read [this](#structure-explanation) to get familiar with the repository, and create a pull request!

### Download Instructions
Want to download? Here's how you can do it.
1. `git clone` this repo (or `git pull` to update if you've already cloned it.)
2. Choose what to download (you can find things in the 'todo' directory in every category)
3. Download it (here's an example command): 
```bash
cat ~/Temp/undownloaded.txt | xargs -I '{}' -P 25 yt-dlp -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/bestvideo+bestaudio' --merge-output-format mp4 --add-metadata --embed-thumbnail --write-thumbnail -i --download-archive ~/youtube-archive/CYOA/downloaded.txt --external-downloader aria2c --external-downloader-args "-j 16 -x 16 -s 16 -k 5M" -o "~/Youtuve/%(channel)s/%(id)s.%(ext)s" --datebefore 20170101 'https://youtube.com/watch?v={}'
```
6. Put the download link in the 'urls.txt' file of your applicable directory
7. `git add .`, `git commit -m "Message"`, `git push` (or create a pull request)

### Useful Commands
Here are some useful commands.

**Filter a video/playlist/ID list for at-risk videos:**

Video/playlist:
```bash
yt-dlp --skip-download --get-id --datebefore 20170101 --match-filter "availability = 'unlisted'" <url>
```
Batch file of URLs:
```bash
yt-dlp --skip-download --get-id --datebefore 20170101 --match-filter "availability = 'unlisted'" --batch-file <file>
```
or if you want to do multiple at once:
```bash
cat <file> | xargs -I '{}' -P 15 yt-dlp --skip-download --get-id --datebefore 20170101 --match-filter "availability = 'unlisted'" 'https://youtube.com/watch?v={}'
```
(you can replace `-P 15` with the number of concurrent instances you want running)

## Structure Explanation
There's a directory for each "flavor" of unlisted videos. For general videos, use the 'general' directory.

Here's an example file structure:
```
|-- General
|   |-- downloaded.txt
|   |-- urls.txt
|   `-- todo
|       |-- list_1.txt
|       |-- list_2.txt
|       `-- random_playlist.txt
```

Each directory has 3 items. 
Here is what they all mean:

#### downloaded.txt
This is a file meant to be used with [`yt-dlp`](https://github.com/yt-dlp/yt-dlp)'s `--download-archive` flag (that's why it has different format than the other files).
[`yt-dlp`](https://github.com/yt-dlp/yt-dlp) uses this file to keep track of already downloaded videos. If it finds a video ID in this file, it doesn't download it.
This is the most important file to keep up-to-date so we stay organized and don't download the same video multiple times.

You can use this command to remove the 'youtube' prefix and make it a list of IDs:
```bash
sed 's/^youtube //g' downloaded.txt >> new_downloaded.txt
```

Remember to `git add`, `git commit`, and `git push` this file every time you download a batch, so we can all stay updated.

#### urls.txt
This is a list of download links for scraped videos.

#### todo
This directory houses files that need to be downloaded. Make sure the files are lists of IDs. 
When you're in the process of downloading one, remove it from the repo so we don't have multiple people getting the same videos.
