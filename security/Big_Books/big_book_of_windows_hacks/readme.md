big book of windows hacks
//compress large files ... 

tar -czvf - /path/to/files | split -b 10M - archive.tar.gz
Will give you a number of files:

archive.tar.gzaa

archive.tar.gzab

...
Which then can be uncompressed with:

cat archive.tar.* | tar -xzvf -
