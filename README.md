# Custom Win64 + CUDA OpenCV Build

## Overview
This is a repo holding a custom build of OpenCV with CUDA support for Windows (64-bit). This is useful if you want to use OpenCV with hardware accelerated Deep Neural Network support using NVidia CUDA on windows. The instructions for building are adapted from the [Build OpenCV on Windows with CUDA!](https://medium.com/@chinssk/build-opencv-on-windows-with-cuda-f880270eadb0) medium blog post (follow along there for pictures). 

The general steps used to create this repo are:
 - Creating an NVidia account
 - Downloading some CUDA installers 
 - Downloading OpenCV Source
 - Copying CUDA libraries to OpenCV source folder
 - Running CMake configuration
 - Waiting an eternity for a build to complete (took almost 24h for me)

## Using Pre-Built Binaries
1. [Download](https://github.com/MikanXR/OpenCVCudaWin64/archive/refs/tags/opencv_4.9.0.zip) and extract the latest `Source Code` zip from the [Releases](https://github.com/MikanXR/OpenCVCudaWin64/releases) page
2. [Download](https://github.com/MikanXR/OpenCVCudaWin64/releases/download/opencv_4.9.0/opencv_world490.dll) and save `opencv_world490.dll` into the extracted `x64\vc17\bin` folder
3. [Download](https://github.com/MikanXR/OpenCVCudaWin64/releases/download/opencv_4.9.0/opencv_world490d.dll) and save `opencv_world490d.dll` into the extracted `x64\vc17\bin` folder
4. Reference the opencv folder in your CMake generation
For example:
`cmake .. -G "Visual Studio 17 2022" -A x64 -DOpenCV_DIR=C:\Github\OpenCVCuda\opencv_cuda`

## Manual Building Binaries

 1. [Download](https://github.com/Kitware/CMake/releases/download/v3.28.1/cmake-3.28.1-windows-x86_64.msi) and install Cmake 3.28
 2. [Download](https://developer.download.nvidia.com/compute/cuda/12.3.2/local_installers/cuda_12.3.2_546.12_windows.exe) CUDA installer
 3. [Download](https://developer.nvidia.com/downloads/compute/cudnn/secure/8.9.6/local_installers/12.x/cudnn-windows-x86_64-8.9.6.50_cuda12-archive.zip/) CuDNN installer
 4. [Download](https://developer.nvidia.com/downloads/designworks/video-codec-sdk/secure/12.1/video_codec_sdk_12.1.14.zip) NVidia Video Codec SDK
 5. [Download](https://github.com/opencv/opencv_contrib/archive/refs/tags/4.9.0.zip) OpenCV 4.9.0 Contrib Source
 6. [Download](https://github.com/opencv/opencv/archive/refs/tags/4.9.0.zip) OpenCV 4.9.0 Source
 7. Run the 12.3.2 CUDA installer
 8. Open a command line in 
 9. Extract cudnn-windows-x86_64-8.9.6.50_cuda12-archive.zip
 10. Copy extracted cuDNN sdk files inside CUDA SDK install location
  `xcopy bin\*.* "C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.3\bin"`
  `xcopy include\*.* "C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.3\include"`
  `xcopy lib\x64\*.* "C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.3\lib\x64"`
  10. Extract the Video_Codec_SDK_12.1.14.zip
  11. Copy the Video SDK files inside CUDA SDK install location
  `xcopy Interface\*.* "C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.3\include"`
  `xcopy lib\x64\*.* "C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.3\lib\x64"`
  12. Add the following paths to the system path
  `“C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.6\bin”`
  `“C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.6\lib”`
  `“C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.6\include”`
  `“C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.6\lib\x64”`
13. Extract opencv-4.9.0.zip
14. Extract opencv_contrib-4.9.0.zip
15. Make a `build` directory in the extracted OpenCV folder
16. Launch the CMake GUI
17. Set "Where is the source code" to OpenCV folder, ex `C:/Github/OpenCVCuda/opencv-4.9.0`
18. Set the build target folder to the build directory you created
19. Hit the configure button
20. Select Visual Studio 17 2022 / X64 / Use Default Native compilers
21. Hit finish and wait for configure
22. Set `OpenCV_EXTRA_MODULES_PATH` to contrib source path, ex: `C:\Github\OpenCVCuda\opencv_contrib-4.9.0\modules`
23. Set `OPENCV_DNN_CUDA` to true
24. Set `WITH_CUDA` to true
25. Set `BUILD_opencv_world` to true
26. `Configure` again
27. If this succeeds, hit `Generate`
28. Open generated sln file, ex: `C:\Github\OpenCVCuda\build\OpenCV.sln`
29. Change build configuration to `Debug` Mode
30. Right click on `CMakeTargets` > `ALL_BUILD` > `Build`
31. Wait an eternity
32. Right click on `CMakeTargets` > `ALL_BUILD` > `Install`
33. Change to `Release` Mode
34. Right click on `CMakeTargets` > `ALL_BUILD` > `Build`
35. Wait another eternity
36. Right click on `CMakeTargets` > `ALL_BUILD` > `Install`
37. Resulting files, which should match the ones checked into this repo, are in the `build\install` folder, ex:  `c:\Github\OpenCVCuda\build\install`
