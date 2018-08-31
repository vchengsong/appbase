

### 修正说明
根目录下的`CMakeLists.txt` 中需要增加`INCLUDE(GNUInstallDirs)` 才能让`CMAKE_INSTALL_FULL_*`获得值，从而让install命令正常运行，
然而将本库用于eosio却不需要添加上述语句，因为eosio根目录的CMakeLists.txt已经添加了上述include语句。
根目录下的`version.cmake.in`中用到了`describe --tags --dirty`，这条命令需要本repo有tag标志，因此需要打tag，才能运行通过，
然而将本库用于eosio却不需要添加上述语句，因为eosio已经有tag。


