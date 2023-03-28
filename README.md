# Fork from https://github.com/alessandro-gentilini/Efficient-RANSAC-for-Point-Cloud-Shape-Detection

## Build
First configure
```
cmake -B cmake-build -S . -T host=x64 -A x64
```
then build
```
cmake --build .\cmake-build\ --config RelWithDebInfo
```

## Changelog
- moved source code to `src` dir
