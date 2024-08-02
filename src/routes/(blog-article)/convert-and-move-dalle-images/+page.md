---
slug: convert-and-move-dalle-images
title: Converting and Moving DALL·E Images with Fish Script
date: 2024-05-08T00:00:00.000Z
excerpt: Learn how to use a Fish shell script to convert DALL·E-generated images from WebP to JPG and manage them efficiently.
coverImage: /images/posts/Conceptual-Workflow.jpg
tags:
  - Fish
  - Shell Script
  - Image Conversion
  - Automation
  - ffmpeg
---

<script>
  import Callout from "$lib/components/molecules/Callout.svelte";
  import CodeBlock from "$lib/components/molecules/CodeBlock.svelte";
  import Image from "$lib/components/atoms/Image.svelte";
</script>

# Introduction

Automating repetitive tasks can save a lot of time and effort, especially when dealing with numerous files. The `convert_and_move_dalle_images` function, written in Fish shell, automates the process of moving DALL·E generated `.webp` images from the Downloads directory to a designated folder, converting them to `.jpg` format, and optionally deleting the original `.webp` files. This post will explain the function step-by-step.

## Function Overview

The `convert_and_move_dalle_images` function performs the following tasks:

1. **Parse arguments**: Checks for a delete flag (`-d` or `--delete`).
2. **Define directories**: Sets up source, destination, and log file paths.
3. **Ensure directories exist**: Creates the necessary directories if they do not exist.
4. **Move files**: Moves `.webp` files from the Downloads folder to the source directory.
5. **Process files**: Converts `.webp` files to `.jpg` format, renames them, and logs the processed files.
6. **Optional deletion**: Deletes the original `.webp` files if the delete flag is set.

Here's the complete function:

<CodeBlock lang="bash">

```bash
function convert_and_move_dalle_images
    # Parse the arguments
    set delete_flag 0
    for arg in $argv
        switch $arg
            case '-d' '--delete'
                set delete_flag 1
        end
    end

    # Store the current directory
    set original_dir (pwd)

    # Define the directories
    set downloads_dir ~/Downloads
    set src_dir ~/Pictures/ai_generated/webp/
    set dst_dir ~/Pictures/ai_generated/jpg/
    set log_file $src_dir/converted_files.log

    # Ensure the source and destination directories exist
    mkdir -p $src_dir
    mkdir -p $dst_dir

    # Ensure the log file exists
    touch $log_file

    # Move .webp files from Downloads to the source directory
    for file in $downloads_dir/*.webp
        if test -f $file
            mv $file $src_dir
        end
    end

    # Change to the source directory
    cd $src_dir

    # Loop through all .webp files in the source directory
    for file in *.webp
        # Check if the file has already been processed
        if not grep -q $file $log_file
            # Extract the base filename without extension
            set base_filename (basename $file .webp)

            # Remove "DALL·E" from the filename if present
            set new_filename (string replace -r "DALL·E" "" $base_filename)

            # Remove date patterns from the filename
            set new_filename (string replace -r "(January|February|March|April|May|June|July|August|September|October|November|December|Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)\s+\d{1,2}" "" $new_filename)
            set new_filename (string replace -r "\d{1,2}/\d{1,2}" "" $new_filename)

            # Trim leading/trailing spaces and hyphens
            set new_filename (string trim -c " -" $new_filename)

            # Replace spaces with dashes
            set new_filename (string replace -a " " "-" $new_filename)

            # Ensure unique filename
            set counter 1
            set final_filename $new_filename
            while test -f $dst_dir$final_filename.jpg
                set final_filename $new_filename"_"$counter
                set counter (math $counter + 1)
            end

            # Convert the .webp file to .jpg using ffmpeg
            ffmpeg -i $file -update 1 -frames:v 1 $dst_dir$final_filename.jpg

            # Log the processed file
            echo $file >> $log_file

            # Delete the original .webp file if the delete flag is set
            if test $delete_flag -eq 1
                rm $file
            end
        end
    end

    # Change back to the original directory
    cd $original_dir
end
```

</CodeBlock>

## Detailed Breakdown

### Argument Parsing

The function first checks if a `-d` or `--delete` argument is provided to determine if the original `.webp` files should be deleted after conversion.

### Directory Setup

It defines and ensures the existence of the following directories and files:

- `downloads_dir`: Where the `.webp` files are initially located.
- `src_dir`: The source directory for processing files.
- `dst_dir`: The destination directory for the converted `.jpg` files.
- `log_file`: A log file to track processed files.

### Moving and Processing Files

1. **Move `.webp` files**: All `.webp` files from the Downloads folder are moved to the source directory.
2. **Convert and Rename**:
   - Removes "DALL·E" and date patterns from filenames.
   - Trims spaces and hyphens.
   - Replaces spaces with dashes.
   - Ensures unique filenames to avoid overwriting.
   - Converts the files using `ffmpeg`.
3. **Logging**: Logs processed files to prevent reprocessing.
4. **Optional Deletion**: Deletes original `.webp` files if the delete flag is set.

### Conclusion

This function simplifies managing and converting your DALL·E generated images, ensuring they are well-organized and consistently named. By automating these tasks, you can focus more on creativity and less on file management. Feel free to customize the function to fit your specific needs and workflow.
