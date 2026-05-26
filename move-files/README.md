# Move Files (Backup Utility)

A Python utility to efficiently back up files between drives or directories. It includes generic folder-to-folder synchronization as well as specialized organization for backing up mobile phone photos (e.g., DCIM directories) to a hard drive.

## Features

- **Incremental Backup**: Only copies files that do not currently exist in the destination folder, saving time and preventing duplicates.
- **Preserves Metadata**: Retains original file creation and modification timestamps using `shutil.copy2`.
- **Smart Phone Photo Organization**: Automatically categorizes photos from `Camera` or `WeChat` folders into subdirectories based on the year they were modified.
- **Modified Photo Handling**: Specifically recognizes edited/filtered photos (e.g., those with `MTXX_` or `MEITU_` prefixes) and places them in a `Year/Modified` subfolder.
- **Detailed Reporting**: Generates a comprehensive text-based backup report summarizing all copied files and verifying if any source files are missing in the destination.

## Prerequisites

- Python 3.x
- No external libraries are required; relies entirely on standard built-in modules (`os`, `shutil`, `datetime`).

## Usage

1. Open `main.py` (or `backup_red.py`) in your preferred text editor.
2. Update the source and destination paths at the bottom of the script according to your needs. 

### Generic Folder Backup

To copy files from a generic source directory to a destination:

```python
SOURCE_ROOT = r"E:/Chen Mobile Pictures"
DEST_ROOT = r"D:/Chen Mobile Pictures"
REPORT_FILE = r"D:/Disk1ToDisk2_backup_report.txt"

backup_files(SOURCE_ROOT, DEST_ROOT, REPORT_FILE)
```

### Phone DCIM Backup

To back up photos from your mobile device and organize them by year:

```python
PHONE_DCIM_FOLDER = r"/path/to/phone/DCIM" 
DISK1_ROOT_FOLDER = r"D:/Chen Mobile Pictures"
PHONE_BACKUP_REPORT_FILE = r"D:/Phone2Disk_backup_report.txt"

backup_phone_dcim(PHONE_DCIM_FOLDER, DISK1_ROOT_FOLDER, PHONE_BACKUP_REPORT_FILE)
```

3. Run the script:

```bash
python main.py
```

4. Once completed, check the generated report file (e.g., `backup_report.txt`) to review the operation logs and verify the backup status.
