A collection of ideas or improvements for Deluge 2.0 release process:


* Add the following to setup.py:
  * ~~Should attempt to minify js scripts and inform user if unsuccessful.~~
  * Update version (and date?) in man files. Eventual view would be to autocreate man files from help options.

* Changes to `make_release` script: 
  * Use `sdist` to create tarball. This replaces use of git archive as sdist should produce a pristine tar so no need to attempt to modify contents as in current `make_release`. 
  * sdist should be able to produce xz tarball with additional python 2.7 module: https://pypi.python.org/pypi/backports.lzma
  * Decide whether need to remove the unwanted extra files such as egg-info.
  * Switch to using just sha256sum for file hash verification and create a `shasums.txt` file.
  * Simplify number of tarballs provided, so either drop gzip or bzip2 (or both).
  * ~~Rename lzma tarball to xz.~~
  * Add `make_release` script to git packaging directory.

* On the ftp, use a single release version folder for all the tarballs, bundled packages and shasum files.