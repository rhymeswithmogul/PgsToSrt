# PgsToSrt

Convert pgs subtitles to srt using OCR.

### Prerequisites
- [.NET 5.0 Runtime](https://dotnet.microsoft.com/download/dotnet/5.0)
- Linux: [Tesseract 3 language data files](https://github.com/tesseract-ocr/tessdata/)

Data files must be placed in a _tessdata_ folder inside PgsToSrt folder, or the path can be specified in the command line with the --tesseractdata parameter.

You only need data files for the language(s) you want to convert.

### Usage

dotnet PgsToSrt.dll [parameters]

| Parameters &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;        |                |
| :---------------      | :------------- |
| --input            | Input filename, can be an mkv file or pgs subtitle extracted to a .sup file with mkvextract.|
| --output           | Output Subrip (.srt) filename. Auto generated from input filename if not set.|
| --track            | Track number of the subtitle to process in an mkv file (only required when input is a matroska file) <br/>This can be obtained with mkvinfo |
| --trackLanguage    | Convert all tracks of the specified language (only works with mkv input)|
| --tesseractlanguage| Tesseract language to use if multiple languages are available in the tesseract data directory.        |
| --tesseractdata    | Path of tesseract language data files, by default 'tessdata' in the executable directory.             |

#### Example (Command Line)
```
dotnet PgsToSrt.dll --input video1.fr.sup --output video1.fr.srt --tesseractlanguage fra
dotnet PgsToSrt.dll --input video1.mkv --output video1.srt --track 4
```
#### Example (Docker)
```
docker run -it --rm -v /data:/data \
           -e INPUT=/data/myImageSubtitle.sup \
           -e OUTPUT=/data/myTextSubtitle.srt \
           -e LANGUAGE=eng \
           tentacule/pgstosrt
```

### Dependencies
- Windows : none, tesseract/leptonica libraries are included in the release package.
- Linux   : libtesseract4 (`sudo apt install libtesseract4`) and tessdata (`git clone https://github.com/tesseract-ocr/tessdata.git --branch 3.04.00`)

### Build
To build PgsToSrt.dll execute this commands
```
dotnet restore
dotnet publish -c Release -o out
#The file is on  PgsToSrt/out/PgsToSrt.dll
```

On macOS or Linux, follow [Microsoft's directions for downloading and installing the .NET runtime](https://docs.microsoft.com/en-us/dotnet/core/install/).  However, be sure to install .NET 5 and not the current version.  For example, after following their Ubuntu instructions, run this command to install the correct version:
```bash
sudo apt install dotnet-runtime-5.0
```

Then, download the Tesseract 3.04 data files.  Do not use the `tesseract-ocr-*` packages from your package manager, as they might be a newer, incompatible version.
```bash
cd out
rm -rf tessdata
git clone https://github.com/tesseract-ocr/tessdata.git --branch 3.04.00
```

(If you are low on disk space, you may delete any unwanted language files from the `tessdata` folder.)


To build docker image (for now only for linux)
```
docker build -t pgstosrt .
```

### Built With
- LibSE from [Subtitle Edit](https://www.nikse.dk/SubtitleEdit/)
- [Tesseract .net wrapper](https://github.com/charlesw/tesseract/)
- [CommandLineParser](https://github.com/commandlineparser/commandline) 
- [SixLabors ImageSharp](https://github.com/SixLabors/ImageSharp)
