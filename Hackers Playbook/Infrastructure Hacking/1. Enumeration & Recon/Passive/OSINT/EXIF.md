Exchangeable image file format (EXIF) is a ***metadata standard that specifies the formats and properties of various media forms***, such as images and voice recordings. EXIF data is ***created by the devices that produce these media items***. The fact that EXIF is a metadata standard also means that the media item can be read by another device with no formatting issues.

[Metadata2Go](hxxps://www.metadata2go.com/) is an online metadata viewer tools.

#### View Metadata
**Display all metadata**
```
exiftool filename.jpg
```

**View specific tags**
```
exiftool -DateTimeOriginal filename.jpg
```

#### Extract Metadata from All files in a Directory
```
exiftool /path/to/directory
```

#### Edit Metadata
**To change metadata**
```
exiftool -DataTimeOriginal="2024:10:30 12:00:00" filename.jpg
```

**Modify multiple files at once**
```
exiftool -Artist="Your Name" *.jpg
```

#### Remove Metadata
**Remove all metadata**
```
exiftool -all= filename.jpg
```

**Remove specific tags**
```
exiftool -GPS:all= filename.jpg
```

#### Export Metadata to a Text File
```
exiftool filename.jpg > metadata.txt
```

#### Copy Metadata from One File to Another
```
exiftool -tagsFromFile source.jpg target.jpg
```