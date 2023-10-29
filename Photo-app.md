# Photo Application

I need to make a console photo app that reads through a list of photos in a folder and sorts the photos based on their date stamp into folders based on **year** then sub folders based on the **month-day** stamp.

Photos are in ``.jpg`` format only.

Photos can be in two formats.

```bash
    20150419_093907.jpg
```

Use the first four digits as the year and the next two digits as the month and the next two digits as the day

Or

```bash
    DSC_1319.JPG
```

For these photos I will use the photo.dll I have to read the datestamp for creating the year and month/day folders.

I will only read the photos from the current folder with no sub folder recursion.

## Steps

Read in files to make a list of photos.

Make a class with properties.

* Filename
* Year 
* Month
* Day

Make a list of unique years. Make year folders.

Make a list of unique month-day values to make sub folders within the respective year folder. Create sub folders.

Iterate through the list of photos and move each photo to their respective month-day folder in the format of **04-17 -**.
