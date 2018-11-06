archiver [![archiver GoDoc](https://img.shields.io/badge/reference-godoc-blue.svg?style=flat-square)](https://godoc.org/github.com/mholt/archiver) [![Linux Build Status](https://img.shields.io/travis/mholt/archiver.svg?style=flat-square&label=linux+build)](https://travis-ci.org/mholt/archiver) [![Windows Build Status](https://img.shields.io/appveyor/ci/mholt/archiver.svg?style=flat-square&label=windows+build)](https://ci.appveyor.com/project/mholt/archiver)
========

Introducing **Archiver 3.0** - a cross-platform, multi-format archive utility and Go library. A powerful and flexible library meets an elegant CLI in this generic replacement for several of platform-specific, format-specific archive utilities.

## Features

Package archiver makes it trivially easy to make and extract common archive formats such as zip and tarball (and its compressed variants). Simply name the input and output file(s). The `arc` command runs the same on all platforms and has no external dependencies (not even libc). It is powered by the Go standard library and several third-party, pure-Go libraries.

Files are put into the root of the archive; directories are recursively added, preserving structure.

- Make whole archives from a list of files
- Open whole archives to a folder
- Extract specific files/folders from archives
- Stream files in and out of archives without needing actual files on disk
- Traverse archive contents without loading them
- Compress files
- Decompress files
- Streaming compression and decompression
- Several archive and compression formats supported

### Format-dependent features

- Optionally create a top-level folder to avoid littering a directory or archive root with files
- Toggle overwrite existing files
- Adjust compression level
- Zip: store (not compress) already-compressed files
- Make all necessary directories
- Open password-protected RAR archives
- Optionally continue with other files after an error

### Supported archive formats

- .zip
- .tar
- .tar.gz or .tgz
- .tar.bz2 or .tbz2
- .tar.xz or .txz
- .tar.lz4 or .tlz4
- .tar.sz or .tsz
- .rar (open only)

### Supported compression formats

- bzip2
- gzip
- lz4
- snappy
- xz


## Install

```bash
go get -u github.com/mholt/archiver/cmd/archiver
```

Or download binaries from the [releases](https://github.com/mholt/archiver/releases) page.


## Command Use

### Make new archive

```bash
# Syntax: arc archive [archive name] [input files...]
$ arc archive test.tar.gz file1.txt images/file2.jpg folder/subfolder
```

(At least one input file is required.)

### Extract entire archive

```bash
# Syntax: arc unarchive [archive name] [destination]
$ arc unarchive test.tar.gz
```

(The destination path is optional; default is current directory.)

The archive name must end with a supported file extension&mdash;this is how it knows what kind of archive to make. Run `arc help` for more help.

### List archive contents

```bash
# Syntax: arc ls [archive name]
$ arc ls caddy_dist.tar.gz
drwxr-xr-x	matt	staff	0		2018-09-19 15:47:18 -0600 MDT	dist/
-rw-r--r--	matt	staff	6148	2017-08-07 18:34:22 -0600 MDT	dist/.DS_Store
-rw-r--r--	matt	staff	22481	2018-09-19 15:47:18 -0600 MDT	dist/CHANGES.txt
-rw-r--r--	matt	staff	17189	2018-09-19 15:47:18 -0600 MDT	dist/EULA.txt
-rw-r--r--	matt	staff	25261	2016-03-07 16:32:00 -0700 MST	dist/LICENSES.txt
-rw-r--r--	matt	staff	1017	2018-09-19 15:47:18 -0600 MDT	dist/README.txt
-rw-r--r--	matt	staff	288		2016-03-21 11:52:38 -0600 MDT	dist/gitcookie.sh.enc
...
```

### Extract a specific file or folder from an archive

```bash
# Syntax: arc extract [archive name] [path in archive] [destination on disk]
$ arc extract test.tar.gz foo/hello.txt extracted/hello.txt
```

### Compress a single file

```bash
# Syntax: arc compress [input file] [output file]
$ arc compress test.txt compressed_test.txt.gz
$ arc compress test.txt gz
```

For convenience, if the output file is simply a compression format (without leading dot), the output file name will be the same as the input name but with the format extension appended, and the input file will be deleted if successful.

### Decompress a single file

```bash
# Syntax: arc decompress [input file] [output file]
$ arc decompress test.txt.gz original_test.txt
$ arc decompress test.txt.gz
```

For convenience, if the output file is not specified, it will have the same name as the input, but with the compression extension stripped from the end, and the input file will be deleted if successful.

### Flags

Flags are specified before the subcommand. Use `arc help` or `arc -h` to get usage help and a description of flags with their default values.

## Library Use

```go
import "github.com/mholt/archiver"
```

The archiver package allows you to easily create and open archives, walk their contents, extract specific files, compress and decompress files, and even stream archives in and out using pure io.Reader and io.Writer interfaces, without ever needing to touch the disk. See [package godoc documentation](https://godoc.org/github.com/mholt/archiver) to learn how to do this -- it's really slick!



## Project Values

This project has a few principle-based goals that guide its development:

- **Do our thing really well.** Our thing is creating, opening, inspecting, compressing, and streaming archive files. It is not meant to be a replacement for specific archive format tools like tar, zip, etc. that have lots of features and customizability. (Some customizability is OK, but not to the extent that it becomes overly complicated or error-prone.)

- **Have good tests.** Changes should be covered by tests.

- **Limit dependencies.** Keep the package lightweight.

- **Pure Go.** This means no cgo or other external/system dependencies. This package should be able to stand on its own and cross-compile easily to any platform -- and that includes its library dependencies.

- **Idiomatic Go.** Keep interfaces small, variable names semantic, vet shows no errors, the linter is generally quiet, etc.

- **Be elegant.** This package should be elegant to use and its code should be elegant when reading and testing. If it doesn't feel good, fix it up.

- **Well-documented.** Use comments prudently; explain why non-obvious code is necessary (and use tests to enforce it). Keep the docs updated, and have examples where helpful.

- **Keep it efficient.** This often means keep it simple. Fast code is valuable.

- **Consensus.** Contributions should ideally be approved by multiple reviewers before being merged. Generally, avoid merging multi-chunk changes that do not go through at least one or two iterations/reviews. Except for trivial changes, PRs are seldom ready to merge right away.

- **Have fun contributing.** Coding is awesome!

We welcome contributions and appreciate your efforts! However, please open issues to discuss any changes before spending the time preparing a pull request. This will save time, reduce frustration, and help coordinate the work. Thank you!
