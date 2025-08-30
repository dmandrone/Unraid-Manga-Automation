<h1 align="center">Unraid Manga Automation Script</h1>

## About The Project

I made this in an effort to automate the process of processing new manga files as they are added to an Unraid server.

Here's what it does:
1. When the script is run, it scans a set of SRC folders defined by the user for any .cbz files.
2. If .cbz files are found it begins to process them.
3. During processing it will do the following:
   * Using the data found in the file(s) [Folder Name, CBZ File Name], it will parse the [Jikan API](https://jikan.moe/) and determine the series that these files belong to.
   * For the matching process, the matcher will prioritize any series results that match the Folder Name exactly, otherwise it will then provide a list of the closest results and provide a score to them based upon how closely it matches the Folder Name. Then, it will select the option with the highest score and apply this information to the files.
   * It will pull the following information from Jikan: Author Name (prioritizes writer over illustrator), Year Published, and Series Name.
   * It will begin to rename the files to match the following format: [Series Name] - Chapter/Volume #.cbz.
     * To expand upoon the above, the processing will always prioritize chapter number. For example, if a file is named 'BECK - Volume 1 Chapter 9.cbz', it will output as 'BECK - Chapter 009.cbz'. If a file is named 'BECK - Volume 1.cbz' it will output as 'BECK - Volume 001.cbz'. Essentially, if a chapter is listed in the file, it will always prioritize that over a volume number to prevent possible metadata issues when transferring to apps such as Kavita.
   * After the files are renamed, it will then move the files to a DEST folder that is defined by the user.
   * It will move them into the following file structure '[Series Name] ([Release Year]) - [Author Name]/[Series Name] - Chapter/Volume #.cbz'.
   * Finally, it will then check if the folder that the unprocessed manga is empty, and delete the folder if it is.
   * It also provides live debug logs during the script run that showcase its thinking process and actions that it is performing.
4. If setting this script to run on a recurring basis in Unraid, it will then processes all of your incoming manga files automatically depending on the cadence that you set.

## Built With

Transparently, this was built "vibe coding" with ChatGPT and Claude.

## Setup

1. Install the 'User Scripts' Unraid Plugin.
2. Navigate to the User Scripts plugin via the 'Settings' page.
3. Click 'Add New Script' and enter a name for the script. I use 'manga-rename'.
4. Paste the script from this repo in the script text window.
5. Save!
6. Set the schedule at which you want this script to automatically run. I have it run once a day at 11pm so that it processes files prior to my 12am daily Kavita scan. Cron for this: 0 23 * * *.
7. Done!

## File Examples

* Source: '/mangaprocessing/Me and the Devil Blues_ The Unreal Life of Robert Johnson/site.com_Ch. 38.cbz' ---> Output: '/manga/Me and the Devil Blues - The Unreal Life of Robert Johnson (2003) - Akira Hiramoto/Me and the Devil Blues - The Unreal Life of Robert Johnson - Chapter 038.cbz'
* Source '/mangaprocessing/Berserk v42 (2025) (Digital) (releasegroup).cbz' ---> Output: '/manga/Berserk (1989) - Kentaro Miura/Berserk - Volume 042.cbz'

## Extra Notes

* It will handle if a file is in a SRC folder without its own folder to encompass it. It will need to read the metadata from the file instead of the folder this time around.
* It will skip all files that do not contain a chapter or volume number (*except for cases listed below).
* It will handle Omnibus/Deluxe/Special edition tags and add those to the final file name should they be included in the original files, even if no chapter or volume number is included in the original file.
* It will handle OneShot tags and appropriately process them even if they do not contain a volume or chapter number.

### There are so many edge cases for manga processing, it probably can't cover of all of the possibilities of errors it could run into. I tried my best to think of as many as possible and make sure they were resolved.

<p align="right">(<a href="#readme-top">back to top</a>)</p>
