# xnxx-dl-tagger

A Python script that recursively updates metadata tags of .mp4 files based on their file names and the directories they reside in. The script sets the Title, Genre, and Release Date tags of each .mp4 file:

Title: Derived from the file name (without the extension).
Genre: Taken from the name of the folder containing the file.
Release Date: Based on the file's last modification date.

Features:
Recursive Processing: Automatically traverses all subdirectories starting from the specified root directory.
Automated Metadata Assignment: Extracts and assigns metadata without manual intervention.
Error Handling: Includes try-except blocks to handle exceptions during file processing.
Non-destructive: Updates metadata in place without moving or altering the original files.

Requirements:
Python: Version 3.6 or higher.
Mutagen Library: A Python module to handle audio metadata.

Installation:
Clone the Repository

bash
git clone https://github.com/your_username/mp4-metadata-updater.git
cd mp4-metadata-updater
Create a Virtual Environment (Optional but Recommended)

bash
python -m venv venv
Activate the Virtual Environment

On Windows:

bash
venv\Scripts\activate
On macOS/Linux:

bash
Code kopiëren
source venv/bin/activate
Install Dependencies

bash
pip install mutagen
Usage
Place the Script in the Desired Directory

Ensure the script update_mp4_metadata.py is located in the root directory containing your .mp4 files and subdirectories.
Run the Script

bash
python update_mp4_metadata.py
Optional: Specify a different root directory

bash
python update_mp4_metadata.py /path/to/your/root_directory
Script Explanation
python
import os
import time
from mutagen.mp4 import MP4, MP4Tags

def create_metadata(root_dir='.'):
    for dirpath, dirnames, filenames in os.walk(root_dir):
        genre = os.path.basename(dirpath)  # Folder name as 'genre'
        for filename in filenames:
            if not filename.lower().endswith('.mp4'):
                continue
            file_path = os.path.join(dirpath, filename)
            name, ext = os.path.splitext(filename)  # File name without extension as 'name'
            mod_time = os.path.getmtime(file_path)
            mod_date = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime(mod_time))

            # Load MP4 file
            try:
                mp4_file = MP4(file_path)
            except Exception as e:
                print(f"Error processing {file_path}: {e}")
                continue

            # Prepare metadata
            mp4_file.tags = mp4_file.tags or MP4Tags()
            mp4_file.tags['\xa9nam'] = name  # Title
            mp4_file.tags['\xa9gen'] = genre  # Genre
            mp4_file.tags['\xa9day'] = mod_date  # Release Date

            # Save tags
            try:
                mp4_file.save()
                print(f"Metadata updated for {file_path}")
            except Exception as e:
                print(f"Error saving {file_path}: {e}")

if __name__ == "__main__":
    create_metadata(root_dir='.')  # Replace '.' with your root directory if needed
Example Directory Structure
markdown
/Media
    /Action
        - Die_Hard.mp4
        - Mad_Max.mp4
    /Comedy
        - Superbad.mp4
        - The_Mask.mp4
After running the script, the metadata for Die_Hard.mp4 will be:

Title: Die_Hard
Genre: Action
Release Date: File's modification date
Troubleshooting
ModuleNotFoundError: No module named 'mutagen.mp4'; 'mutagen' is not a package
Cause: This error occurs when Python cannot find the mutagen library or there's a naming conflict.

Solution:

Ensure mutagen is Installed

bash
pip install mutagen
Check for Naming Conflicts

Make sure there's no file named mutagen.py or a directory named mutagen in your project directory.
Delete any __pycache__ directories if present.
Verify Python Environment

Confirm you're using the correct Python interpreter where mutagen is installed.
Use virtual environments to manage dependencies.
Test Imports in Python Shell

bash
python
python
import mutagen
from mutagen.mp4 import MP4, MP4Tags
If this works without errors, the environment is set up correctly.
Multiple Python Versions
If you have multiple Python versions installed, ensure that pip and python refer to the same version.

Check Python Version

bash
python --version
Check Pip Version

bash
pip --version
Contributing
Contributions are welcome! Please open an issue or submit a pull request for any enhancements or bug fixes.

Fork the Repository

Create a Feature Branch

bash
git checkout -b feature/YourFeature
Commit Your Changes

bash
git commit -m 'Add YourFeature'
Push to the Branch

bash
git push origin feature/YourFeature
Open a Pull Request

License
This project is licensed under the MIT License.

Acknowledgements
Mutagen Library: For handling audio metadata in Python.
Contact
Author: Joannes Wyckmans
Email: none
Feel free to reach out with questions or feedback!
