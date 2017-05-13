# incfilebackup
A simple backup script creating incremental copies of folders using hardlinks

The usage of this script is as simple as it gets:
# Ensure that you have an up-to-date version of Java 8 installed
# Download it (there will be no versions for now, just use githubs download feature)
# Copy everything from the dist folder where you want to keep the script, i.e. YOUR_HOME_FOLDER/incbackup
# Check if the backup.bat script suits your needs
# Check if the backup-json.config suits your needs, customize especially source and target folder
# Make a test run

Note:
I'm plannig to remove the config file soon, instead relying on system properties for customization which you can customize in your script.
Sources and targets will become normal commandline arguments then, which makes more sense anyway.



