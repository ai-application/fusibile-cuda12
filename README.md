# How to build fusibile under new opencv and cuda version
# The original code is from: https://github.com/kysucix/fusibile. Thanks the author's contributions in Multi-view Stereo field.
1. download the opencv source code, and push into the master root path, like this:
   ![image](https://github.com/user-attachments/assets/8843247c-3b04-4731-b4ba-97e3c12d11c6)
   ![image](https://github.com/user-attachments/assets/fdcc8bf8-01c2-4fa5-896f-951865dc0516)
3. Build the opencv code and generate the so libs, enter the opencv "sources" directory and mkdir build path, enter the build path:
   a. rm -fm build
   b. mkdir build
   c. cd build
   d. cmake -D BUILD_opencv_dnn=OFF -D WITH_FFMPEG=OFF ..
   e. sudo make -j4
   f. sudo make install
4. modify your CMakeLists.txt file, change to your own cuda version and g++ version. For example, check the usr/local path, my cuda version is 12.2.
   ![image](https://github.com/user-attachments/assets/eeec9bd3-8034-4822-b7a0-b504717914ec)
5. modify the file to your corresponding cuda version, like this:
   ![image](https://github.com/user-attachments/assets/21d72822-72bc-4408-b395-ee6b39eb043d)
6. cmake .
7. make
8. you can obtain the following success!!!
   ![image](https://github.com/user-attachments/assets/32524f96-8534-4d13-b8ac-c08b5ee8f749)
