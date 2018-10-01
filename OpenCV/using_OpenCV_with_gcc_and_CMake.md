# Using OpenCV with gcc and CMake
#opencv

[Using OpenCV with gcc and CMake — OpenCV 2.4.13.7 documentation](https://docs.opencv.org/2.4/doc/tutorials/introduction/linux_gcc_cmake/linux_gcc_cmake.html)

* The easiest way of using OpenCV in your code is to use CMake. A few advantages (taken from the Wiki):
	* No need to change anything when porting between Linux and Windows.
	* Can easily be combined with other tools by CMake(i.e. Qt, ITK and VTK)

Test Simple Program using OpenCV
```cpp
#include <stdio.h>
#include <opencv2/opencv.hpp>

using namespace cv;

int main(int argc, char** argv )
{
    if ( argc != 2 )
    {
        printf("usage: DisplayImage.out <Image_Path>\n");
        return -1;
    }

    Mat image;
    image = imread( argv[1], 1 );

    if ( !image.data )
    {
        printf("No image data \n");
        return -1;
    }
    namedWindow("Display Image", WINDOW_AUTOSIZE );
    imshow("Display Image", image);

    waitKey(0);

    return 0;
}
```

## Create a CMake file
사실상 나에게 가장 중요했던 파일, 이 정보를 찾기 위해 수많은 삽질을 감행했었다.
```makefile
cmake_minimum_required(VERSION 2.8)
project( DisplayImage )
find_package( OpenCV REQUIRED )
add_executable( DisplayImage DisplayImage.cpp )
target_link_libraries( DisplayImage ${OpenCV_LIBS} )
```

week3라는 프로젝트에 적용한 내 CMakeLists.txt 파일의 내용은 아래와 같다.
```makefile
cmake_minimum_required(VERSION 3.12)
project(week3)

set(CMAKE_CXX_STANDARD 14)

find_package( OpenCV REQUIRED )

add_executable(week3 main.cpp)

target_link_libraries( week3 ${OpenCV_LIBS} )
```

## Generate the executable
```bash
cd <DisplayImage_directory>
cmake .
make
```

## Result
By now you should have an executable (called DisplayImage in this case). You just have to run it giving an image location as an argument, i.e.:

`./DisplayImage lena.jpg`
You should get a nice window as the one shown below:


![](using_OpenCV_with_gcc_and_CMake/F46297A9-31DD-4B72-A594-EF892920A83E.png)

