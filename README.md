# http-server-upload

[![NPM version](https://img.shields.io/npm/v/http-server-upload.svg)](https://www.npmjs.com/package/http-server-upload)
[![Downloads](https://img.shields.io/npm/dm/http-server-upload.svg)](https://www.npmjs.com/package/http-server-upload)

[![NPM](https://nodei.co/npm/http-server-upload.png?downloads=true)](https://nodei.co/npm/http-server-upload/)

This is a simple zero-configuration command-line http server which provides a lightweight interface to upload files.

By default files are uploaded to the current working directory.

Optionally a token may be used to protect against unauthorized uploads.


## Installation

```
npm install --global http-server-upload
```

This will install `http-server-upload` globally so that it may be run from the command line.


## Usage

```
http-server-upload [arguments] [uploadRootPath]
```

`[uploadRootPath]` defaults to the current working directory (`./`).

Other options can be set using environment variables.

When the server is running you can visit http://localhost:8080/ to get the upload form.

If the desired port is already in use, the port will be increased automatically
until the next free port is found. This can be disabled, see below.

*Attention:* Already existing files will be overwritten on upload.


### Arguments and environment variables

The optional configuration is done by command line arguments or environment variables.  
If both are used, the arguments have higher priority and the value from the
corresponding environment variable will be ignored.

| Argument | Variable | Description | Default |
|---|---|---|---|
| `--port` | `PORT` | The port to use. | `8080` |
| `--upload-dir` | `UPLOAD_DIR` | The directory where the files should be uploaded to. This overrides the `uploadRootPath` argument. | `uploadRootPath` argument or the current working directory |
| `--upload-tmp-dir` | `UPLOAD_TMP_DIR` | Temp directory for the file upload. | The upload directory. |
| `--max-file-size` | `MAX_FILE_SIZE` | The maximum allowed file size for uploads in Megabyte. | `200` |
| `--token` | `TOKEN` | An optional token which must be provided on upload. | Nothing |
| `--path-regexp` | `PATH_REGEXP` | A regular expression to verify a given upload path. This should be set with care, because it may allow write access to outside the upload directory. | `/^[a-zA-Z0-9-_/]*$/` |
| `--disable-auto-port` | `DISABLE_AUTO_PORT` | Disable automatic port increase if the port is nor available. | Not set. |
| `--help`, `-h` | | Show some help text | |

Examples:
```
PORT=9000 UPLOAD_DIR=~/uploads/ UPLOAD_TMP_DIR=/tmp/ TOKEN=my-super-secret-token http-server-upload

http-server-upload --port=9000 --upload-dir="c:\users\peter\Path With Whitespaces\"

PORT=9000 http-server-upload --disable-auto-port ./
```


### Uploads from the command line

If the `http-server-upload` is running, you may also upload files from the command line using `curl`:
```
curl -F "uploads=@my-file.txt" http://localhost:8080/upload
```

Advanced example with multiple files, an upload path and a required token:
```
curl \
  -F "uploads=@my-file.txt" \
  -F "uploads=@my-other-file.txt" \
  -F "path=my/dir" \
  -F "token=my-super-secret-token" \
  http://localhost:8080/upload
```


## License

MIT license

Copyright (c) 2019-2022 Peter Müller <peter@crycode.de> https://crycode.de
