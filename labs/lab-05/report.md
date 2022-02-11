## Step 1
**tutorial.cxx**

`
// A simple program that computes the square root of a number
#include "TutorialConfig.h"
#include <cmath>
#include <iostream>
#include <string>

int main(int argc, char* argv[])
{
  if (argc < 2) {
    // report version
    std::cout << argv[0] << " Version " << Tutorial_VERSION_MAJOR << "."
              << Tutorial_VERSION_MINOR << std::endl;
   std::cout << "Usage: " << argv[0] << " number" << std::endl;
   return  1;
  }

  // convert input to double
  const double inputValue = std::stod(argv[1]);

  // calculate square root
  const double outputValue = sqrt(inputValue);
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
  return 0;
}

`

**CMakeLists.txt**

`
cmake_minimum_required(VERSION 3.10)

project(Tutorial VERSION 1.0)

configure_file(TutorialConfig.h.in TutorialConfig.h)

#specify the C++ standatd
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_executable(Tutorial tutorial.cxx)


target_include_directories(Tutorial PUBLIC
	                   "${PROJECT_BINARY_DIR}"
			   )
`


![image](https://user-images.githubusercontent.com/48782723/153648164-5564d411-e230-4bd5-b9c4-4e4fed6dd99c.png)
 
