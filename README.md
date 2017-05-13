# incfilebackup
A simple backup script creating incremental copies of folders using hardlinks

It follows a very simple algorithm: The base idead is to copy all files from source to target directory, in which a subdir for the current day is created. This ensures that each day dir has it's on full copy of the files. A disadvantage of this approach however is that its slow and consumes countless GB of memory for each backup, even if the source has never changed. On the other hand its simple an easy to understand: If you want to use a backup, then just copy the contents of the day  folder in question.

To combat the slow backup time and disk usage issue, the script first checks for each file if there was already a backup for it made. If there is, then its size and last modified timestamp are checked. If both are identical, the source file will not be copied. Instead, a so called 'hard link' is created in the backup dir which uses the same file contents as a previously backupped file - without requiring any more memory. A disadvantage of this approach is that you must never change a file in backup, because if you change one backup, then all of its linked other backups are changed as well... But that is rarely a problem with real backups, because if you want to restore a backup you will nearly always just copy all files from the day folder of your choice (usually the last) anyway.

IncBackup is really fast if you're creating a backup even of a complete harddisk as long as only very few files are changed: On a slow drive, a few thousand links of older files can created EACH SECOND. Since most large files are those which rarely change (say, videos or photos), you end up with a backup system which is simple, portable and usally fast than most commercial solutions.

The only area in which this script currently fails are renamed or moved files: Those cannot be detected and will always enforce a full copy of all affected files. In the future this problem can be solved by using so called checksums to identify moves, but for now this is out of scope. (And if it happens you can just delete on of the old backups anyways, which is rarely a problem)


The usage of this script is as simple as it gets:

1. Ensure that you have an up-to-date version of Java 8 installed
2. Download it (there will be no versions for now, just use githubs download feature)
3. Copy everything from the dist folder where you want to keep the script, i.e. YOUR_HOME_FOLDER/incbackup
4. Check if the backup.bat script suits your needs
5. Check if the backup-json.config suits your needs, customize especially source and target folder
6. Make a test run

Note:
I'm plannig to remove the config file soon, instead relying on system properties for customization which you can customize in your script.
Sources and targets will become normal commandline arguments then, which makes more sense anyway.


FAQS


What is the main usecase of this script?
- Backups of personal files, even entire drives. Do not use this script to created system backups to restore your operating system. The algorithm presented here pretty much fails in this usecase. There are many better ways to create system backups in the OS like windows. However, there are few ways which are as elegant and simple to create backup or your personal files.

Are there size limitations?
- No. In fact, the script might be most useful if you use it on extremely large files which nearly never change like photos or videos. Just remind that currently moving or renaming will always trigger a full copy.

What happens if a file has the same last modified date AND size but was changed?
- BAD THINGS. Got me there. These files will be linked even if the content is not the same. On the other hand, this really sounds like a very malicious way to change a file anyway, so not creating a backup is pretty ok this scenario. What happens here is that some crazy programmer has somehow hacked the last modified date after the writing, and I am confident enough to skip these files.

Can I safely delete OLD backup dirs? What will happen to the files in them which are linked in newer backups?
- You can safely delete them. Even if the file CONTENTs are linked, the files are not. If you delete an older backup folder, only those file contents are lost which are not part of any of the newer backups. Delete a file which has multiple hardlinks never affects the other hardlinks.

Can I modify files in the backup folders?
- NO, you shouldn't. The are intended for making copies of them only. Modifying will affect all other backup folders which link to this file. As I said, the CONTENTs exist only once, only the file entries are duplicated which reference these contents.

Can I safely work while the backup is created?
- NO, you shouldn't modify any of the files for which a backup is currently created. This can have unexpected results, even if thosre are usually migitated with the next backup round. Just be careful and start it before you turn off your computer, ok?

