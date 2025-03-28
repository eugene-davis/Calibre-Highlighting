## Technical Documentation for Book Highlight Mapping Program

This document provides an overview of a Python program designed to automate the management of book highlights. The program facilitates the parsing of highlights from Kindle devices, matching these highlights to books in the Calibre library, and further handling the matched data for user interactions. This comprehensive tool is particularly useful for individuals looking to streamline their digital reading workflow and synchronize their highlights across different platforms.

### Program Overview

The program consists of several key functions that work together to parse highlight data, match it to a digital library, and interact with the user for verification and viewing purposes. Each part of the program leverages Python's powerful libraries to handle file I/O, string manipulation, regular expressions, and subprocess management.

### Modules and Libraries Used

1. **JSON**: Used for parsing and saving data in JSON format, which is a lightweight data interchange format.
2. **re (Regular Expressions)**: Utilized to manipulate strings by replacing special characters and formatting strings as needed.
3. **subprocess**: Enables interaction with the system to run command-line commands, specifically to interact with the Calibre database and viewer.
4. **fuzzywuzzy**: Implements fuzzy logic for matching text strings, which helps in aligning titles from Kindle highlights to those in the Calibre database.
5. **pyautogui**: Automates GUI interactions for searching and highlighting texts within the Calibre viewer.
6. **pyperclip**: Manages the system clipboard to copy and paste text during automated GUI interactions.
7. **time**: Provides timing functions to manage delays in automation sequences to ensure synchronization with GUI responses.

### Function Descriptions

1. **parse_clippings(file_path, remove_duplicates)**:
   - **Purpose**: Parses the 'My Clippings.txt' from a Kindle device, extracting book titles, locations, timestamps, and highlights.
   - **Outputs**: A JSON file named 'parsed_highlights.json', containing structured highlight data organized by book title.

2. **replace_special_characters(text, replacement_char)**:
   - **Purpose**: Cleans and prepares text by replacing all non-alphanumeric characters (except spaces, dashes, and brackets) with a specified character, typically an underscore.
   - **Outputs**: A string with special characters replaced, suitable for consistent text matching.

3. **read_calibre_device_metadata(path)**:
   - **Purpose**: Reads metadata from a JSON file that maps Kindle device book titles to Calibre library IDs.
   - **Outputs**: A dictionary with cleaned book titles as keys and corresponding Calibre IDs as values.

4. **list_all_books_and_save()**:
   - **Purpose**: Fetches a list of all books from the Calibre library via the `calibredb.exe` CLI tool and saves this list to 'calibre_books.json'.
   - **Outputs**: A boolean indicating success or failure, with the data saved to a JSON file.

5. **find_best_match_book_id_and_save(highlights_path, calibre_books_path)**:
   - **Purpose**: Matches titles from parsed highlights to Calibre book data using fuzzy matching and saves detailed mapping information to 'device_to_calibre_mapping.json'.
   - **Outputs**: None directly, but outputs a JSON file with comprehensive mapping results.

6. **open_book_in_calibre_viewer(book_path)**:
   - **Purpose**: Opens a book in the Calibre viewer, allowing for manual or automated review of matched highlights.
   - **Outputs**: None, operates as a system command to open the viewer.

7. **perform_text_operations(highlight_texts)**:
   - **Purpose**: Automates the search and highlighting of specific texts within the Calibre viewer using GUI automation tools.
   - **Outputs**: A list of highlights that were not found or correctly confirmed by the user.

### Workflow

1. **Data Parsing**: The program starts by parsing highlight data from a Kindle device.
2. **Data Matching**: Using the parsed data and metadata from the Calibre library, the program performs fuzzy matching to align highlights with library books.
3. **User Interaction**: Through GUI automation, the program facilitates the viewing and manual confirmation of these highlights within the Calibre viewer.
4. **Output Generation**: Throughout the process, structured data is saved to JSON files for persistence and further analysis.

### Tips

1. If your book in Calibre has more than one version or format, ex. EPUB and MOBI, you could inform the program to which version to use by adjusting its path in `device_to_calibre_mapping.json` to be the first in title.

```JSON
[
   {
      ...
      ...
      ...
      "book_path": [
         "path/to/book.mobi"
         "path/to/book.epub",
        ],
   }
]
```
If you would like to Highlight the EPUB version, reorder it to be the first path, like below

```JSON
[
   {
      ...
      ...
      ...
      "book_path": [
         "path/to/book.epub",
         "path/to/book.mobi"
        ],
   }
]
```
2. You can manually specify the path to `calibredb` with the environmental variable `CALIBREDB_BIN`, e.g. `CALIBREDB_BIN=/usr/bin/calibredb python main.py`. By default it will by `calibredb` or for Windows `calibredb.exe`.
3. You can disable the manual approval of each highlight with the environmental variable `HIGHLIGHT_AUTO_APPLY`, e.g. `HIGHLIGHT_AUTO_APPLY=true python main.py` By default it is enabled.
4. If you use `HIGHLIGHT_AUTO_APPLY` some or all highlights are missing, try setting `HIGHLIGHT_AUTO_APPLY_DEFAULT_SLEEP` to a value greater than one (second), e.g. `HIGHLIGHT_AUTO_APPLY_DEFAULT_SLEEP=2 HIGHLIGHT_AUTO_APPLY=true main.py`. This will increase the duration between each highlight, in case Calibre cannot keep up.

### Future Enhancements

1. **User Interface**: Develop a GUI to make the program more accessible to non-technical users.
2. **Performance Optimization**: Optimize file reading and writing operations and improve the efficiency of string manipulations and regex operations.
3. **Expanded Compatibility**: Extend compatibility to more devices and formats, potentially integrating direct eBook formats beyond Kindle.

