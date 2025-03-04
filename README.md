# FileManagerAutomater README

FileManagerAutomater is a Python project designed to automate the process of organizing and compressing files. It scans a designated input folder for files, categorizes them based on their type (e.g., PDFs, images), moves them into corresponding subfolders in an output directory, and applies compression using external services (ConvertAPI for PDFs and TinyPNG for images). This project helps you maintain an organized file system while reducing file sizes for easier storage and transfer.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Project Structure](#project-structure)
- [Sample Output](#sample-output)
- [Detailed Workflow](#detailed-workflow)
  - [1. Loading Configurations](#1-loading-configurations)
  - [2. Scanning and Moving Files](#2-scanning-and-moving-files)
  - [3. Compressing Files](#3-compressing-files)
- [Module Descriptions](#module-descriptions)
  - [agents.py](#agentspy)
- [Setup and Installation](#setup-and-installation)
- [Usage Examples](#usage-examples)
- [Summary](#summary)

---

## Project Overview

FileManagerAutomater automates file management tasks by processing files in a specified input directory and organizing them in an output directory. The program performs the following tasks:
- **Scanning:** Recursively searches the input folder for files.
- **Categorization:** Identifies file types based on file extensions (e.g., PDFs, images).
- **Organization:** Moves files into subdirectories within the output folder based on their category.
- **Compression:** Uses ConvertAPI to compress PDF files and TinyPNG to compress image files.

By automating these processes, the project minimizes manual file handling and helps optimize file storage space.

---

## Project Structure

Below is the directory structure for FileManagerAutomater:

- **README.md:** This document provides an overview and instructions for the project.
- **agents.py:** The main Python script that implements file scanning, categorization, moving, and compression.
- **agentic_config.yml:** YAML configuration file containing the paths for the input and output folders.
- **file_manager_config.yml:** YAML configuration file that specifies the compression methods for different file types.
- **data/**: Contains two subdirectories:
  - **input_folder:** Where the original files are stored.
  - **output_folder:** Where files are moved and organized.
- **requirements.txt:** Lists the required Python packages needed to run the project.

---

## Sample Output

When you run the script, the output in the terminal will be similar to:

![Sample Output](<Screenshot 2025-03-04 at 22.41.55.png>)

This output provides detailed feedback for each step: the original file size, confirmation of file movement, the decision process for selecting the compression function, and the result after compression.

---

## Detailed Workflow

### 1. Loading Configurations

At startup, the program loads configuration files using the `load_config` function. There are two configuration files:

- **agentic_config.yml:**  
  This file contains paths for the input and output folders. For example:
  ```yaml
  input_folder_path: "data/input_folder"
  output_folder_path: "data/output_folder"
  ```
  The script reads these values to know where to find the files and where to place them after processing.

- **file_manager_config.yml:**  
  This file defines the compression methods for various file types:
  ```yaml
  compression_method:
    - PDF: "convertapi"
    - JPG: "tinypng"
    - PNG: "tinypng"
  ```
  These settings tell the program which external API to use for compressing PDFs and images.

By loading these configurations at the beginning, the project ensures that all file paths and compression methods are easily adjustable without changing the code.

### 2. Scanning and Moving Files

The program then scans the specified input folder recursively to locate all files. This is achieved with the `scan_folder` function. Here’s what happens:

- **Scanning:**  
  The function walks through every subdirectory of the input folder and collects file paths. This means it can handle nested directories, ensuring no file is overlooked.

- **Categorization:**  
  For each file, the `get_file_category` function checks its file extension:
  - `.pdf` files are labeled as **PDFs**.
  - Image files (e.g., `.png`, `.jpg`, `.jpeg`) are labeled as **Images**.
  - Other file types fall into the **Others** category.

- **Moving Files:**  
  Once the file category is determined, the `move_file_to_category` function moves the file into a corresponding subfolder within the output directory. For example, an image file is moved to:
  ```
  data/output_folder/Images/
  ```
  This function not only moves the file but also prints confirmation messages, such as the original size and the new file location. This feedback helps you verify that files are being correctly organized.

### 3. Compressing Files

After moving each file, the script checks if the file qualifies for compression:

- **Selecting Compression Function:**  
  The function `select_compression_function` determines the right compression function based on the file category:
  - For **PDFs**, it selects `compress_pdf`.
  - For **Images**, it selects `compress_image`.
  - For other categories, no compression is applied.

- **Compression Process:**  
  - **PDF Compression:**  
    The `compress_pdf` function sends the PDF file to ConvertAPI using an HTTP POST request. It includes the file, a secret API key, and additional parameters. Upon a successful response, it prints a success message.
  - **Image Compression:**  
    The `compress_image` function uses the TinyPNG API via the `tinify` Python package. It uploads the image, compresses it, and saves the result back to the same location. If successful, it prints a message confirming the compression.

Throughout the compression phase, the program prints the file sizes before and after compression. This detailed output lets you see the efficiency of the compression process.

---

## Module Descriptions

### agents.py

- **Purpose:**  
  The `agents.py` script is the central module that coordinates the file management workflow. It integrates configuration loading, file scanning, file movement, and file compression into one seamless process.

- **Key Functions:**
  - `load_config(config_path)`: Reads YAML configuration files to fetch input/output paths and compression methods.
  - `scan_folder(folder_path)`: Recursively collects all file paths from the input folder.
  - `get_file_category(file_path)`: Determines the category of a file based on its extension (e.g., PDFs, Images).
  - `move_file_to_category(file_path, base_output_folder)`: Moves a file into the appropriate subdirectory within the output folder.
  - `compress_pdf(file_path, method)`: Compresses PDF files using ConvertAPI.
  - `compress_image(file_path, method)`: Compresses image files using TinyPNG.
  - `select_compression_function(category)`: Chooses the appropriate compression function based on the file category.
  - `main()`: The main driver function that orchestrates the entire process—loading configurations, scanning files, moving them, and compressing as necessary.

This modular design ensures that each task is handled by a dedicated function, making the code easier to maintain and update.

---

## Setup and Installation

1. **Clone or Download the Repository:**  
   Make sure you have the complete project structure, including configuration files and the `data` folder.

2. **Create a Virtual Environment:**  
   It is recommended to use a virtual environment to manage dependencies:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. **Install Required Packages:**  
   Install the required packages using the provided `requirements.txt`:
   ```bash
   pip install -r requirements.txt
   ```

4. **Configuration Files:**  
   Ensure that `agentic_config.yml` and `file_manager_config.yml` are correctly set up with your desired paths and compression methods.

5. **API Keys and Dependencies:**  
   - For PDF compression, ensure you have a valid ConvertAPI secret key.
   - For image compression, install the `tinify` package and use your TinyPNG key.

---

## Usage Examples

### Example 1: Processing Image Files

Assume you have the following image files in your input folder:
- `imagesample1.png`
- `imagesample2.png`
- `imagesample3.png`

When you run the script with:
```bash
python3 agents.py
```
The program will:
- Scan the input folder and list these image files.
- Determine that these files belong to the **Images** category.
- Move each file into `data/output_folder/Images`.
- Select the TinyPNG compression function to compress each image.
- Print the original and compressed file sizes, along with success messages.

### Example 2: Processing PDF Files

If your input folder contains:
- `pdfsample1.pdf`
- `pdfsample2.pdf`

The script will:
- Recognize these files as **PDFs**.
- Move them to `data/output_folder/PDFs`.
- Use the ConvertAPI service to compress the PDF files.
- Display messages that show the file sizes before and after compression, ensuring that your PDFs are optimized.

---

## Summary

FileManagerAutomater is a comprehensive tool for automating file organization and compression. By leveraging configuration files and external APIs, it eliminates the need for manual file management tasks. The project’s modular structure—with functions dedicated to scanning, moving, and compressing files—ensures that each component is easy to understand, maintain, and modify.

With detailed output at every step, you receive immediate feedback on file movements and compression efficiency. This allows you to verify that the system works correctly and to quickly identify any issues. Whether you need to manage a few files or handle large volumes of data, FileManagerAutomater streamlines the process and optimizes storage with minimal manual intervention.

Enjoy using FileManagerAutomater to keep your files organized and efficient!
