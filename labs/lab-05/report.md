## Step 1

![image](https://user-images.githubusercontent.com/48782723/153648164-5564d411-e230-4bd5-b9c4-4e4fed6dd99c.png)


**tutorial.cxx**

```
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

```

**CMakeLists.txt**

```
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
```

## Step 2

![image](https://user-images.githubusercontent.com/48782723/153729349-26c797be-27fe-4716-84b9-e2a2acbe031a.png)

**tutorial.cxx**

```
```


**CMakeLists.txt**

```
```


## Step 3

![image](https://user-images.githubusercontent.com/48782723/153729912-c3f2c220-830b-4827-8dbf-2e5c0c5168d6.png)


**CMakeLists.txt**

```
```

**MathFunctions/CMakeLists.txt**

```
```


## Step 4

**CMakeLists.txt**

```
```

**MathFunction/CMakeLists.txt**

```
```

**Output of cmake -VV**
```
abagramian@DESKTOP-4VD6VS6:/mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build$ ctest -VV
UpdateCTestConfiguration  from :/mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/DartConfiguration.tcl
UpdateCTestConfiguration  from :/mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/DartConfiguration.tcl
Test project /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build
Constructing a list of tests
Done constructing a list of tests
Updating test list for fixtures
Added 0 tests to meet fixture requirements
Checking test dependency graph...
Checking test dependency graph end
test 1
    Start 1: Runs

1: Test command: /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/Tutorial "25"
1: Test timeout computed to be: 9.99988e+06
1: Computing sqrt of 25 to be 13
1: Computing sqrt of 25 to be 7.46154
1: Computing sqrt of 25 to be 5.40603
1: Computing sqrt of 25 to be 5.01525
1: Computing sqrt of 25 to be 5.00002
1: Computing sqrt of 25 to be 5
1: Computing sqrt of 25 to be 5
1: Computing sqrt of 25 to be 5
1: Computing sqrt of 25 to be 5
1: Computing sqrt of 25 to be 5
1: The square root of 25 is 5
1/9 Test #1: Runs .............................   Passed    0.01 sec
test 2
    Start 2: Usage

2: Test command: /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/Tutorial
2: Test timeout computed to be: 9.99988e+06
2: /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/Tutorial Version 1.0
2: Usage: /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/Tutorial number
2/9 Test #2: Usage ............................   Passed    0.01 sec
test 3
    Start 3: Comp4

3: Test command: /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/Tutorial "4"
3: Test timeout computed to be: 9.99988e+06
3: Computing sqrt of 4 to be 2.5
3: Computing sqrt of 4 to be 2.05
3: Computing sqrt of 4 to be 2.00061
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: Computing sqrt of 4 to be 2
3: The square root of 4 is 2
3/9 Test #3: Comp4 ............................   Passed    0.01 sec
test 4
    Start 4: Comp9

4: Test command: /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/Tutorial "9"
4: Test timeout computed to be: 9.99988e+06
4: Computing sqrt of 9 to be 5
4: Computing sqrt of 9 to be 3.4
4: Computing sqrt of 9 to be 3.02353
4: Computing sqrt of 9 to be 3.00009
4: Computing sqrt of 9 to be 3
4: Computing sqrt of 9 to be 3
4: Computing sqrt of 9 to be 3
4: Computing sqrt of 9 to be 3
4: Computing sqrt of 9 to be 3
4: Computing sqrt of 9 to be 3
4: The square root of 9 is 3
4/9 Test #4: Comp9 ............................   Passed    0.01 sec
test 5
    Start 5: Comp5

5: Test command: /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/Tutorial "5"
5: Test timeout computed to be: 9.99988e+06
5: Computing sqrt of 5 to be 3
5: Computing sqrt of 5 to be 2.33333
5: Computing sqrt of 5 to be 2.2381
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: Computing sqrt of 5 to be 2.23607
5: The square root of 5 is 2.23607
5/9 Test #5: Comp5 ............................   Passed    0.01 sec
test 6
    Start 6: Comp7

6: Test command: /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/Tutorial "7"
6: Test timeout computed to be: 9.99988e+06
6: Computing sqrt of 7 to be 4
6: Computing sqrt of 7 to be 2.875
6: Computing sqrt of 7 to be 2.65489
6: Computing sqrt of 7 to be 2.64577
6: Computing sqrt of 7 to be 2.64575
6: Computing sqrt of 7 to be 2.64575
6: Computing sqrt of 7 to be 2.64575
6: Computing sqrt of 7 to be 2.64575
6: Computing sqrt of 7 to be 2.64575
6: Computing sqrt of 7 to be 2.64575
6: The square root of 7 is 2.64575
6/9 Test #6: Comp7 ............................   Passed    0.01 sec
test 7
    Start 7: Comp25

7: Test command: /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/Tutorial "25"
7: Test timeout computed to be: 9.99988e+06
7: Computing sqrt of 25 to be 13
7: Computing sqrt of 25 to be 7.46154
7: Computing sqrt of 25 to be 5.40603
7: Computing sqrt of 25 to be 5.01525
7: Computing sqrt of 25 to be 5.00002
7: Computing sqrt of 25 to be 5
7: Computing sqrt of 25 to be 5
7: Computing sqrt of 25 to be 5
7: Computing sqrt of 25 to be 5
7: Computing sqrt of 25 to be 5
7: The square root of 25 is 5
7/9 Test #7: Comp25 ...........................   Passed    0.01 sec
test 8
    Start 8: Comp-25

8: Test command: /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/Tutorial "-25"
8: Test timeout computed to be: 9.99988e+06
8: The square root of -25 is 0
8/9 Test #8: Comp-25 ..........................   Passed    0.01 sec
test 9
    Start 9: Comp0.0001

9: Test command: /mnt/c/Users/alexi/OneDrive/Documents/RPI/Spring22/OSS/lab5/cmake/Help/guide/tutorial/Step4_build/Tutorial "0.0001"
9: Test timeout computed to be: 9.99988e+06
9: Computing sqrt of 0.0001 to be 0.50005
9: Computing sqrt of 0.0001 to be 0.250125
9: Computing sqrt of 0.0001 to be 0.125262
9: Computing sqrt of 0.0001 to be 0.0630304
9: Computing sqrt of 0.0001 to be 0.0323084
9: Computing sqrt of 0.0001 to be 0.0177018
9: Computing sqrt of 0.0001 to be 0.0116755
9: Computing sqrt of 0.0001 to be 0.0101202
9: Computing sqrt of 0.0001 to be 0.0100007
9: Computing sqrt of 0.0001 to be 0.01
9: The square root of 0.0001 is 0.01
9/9 Test #9: Comp0.0001 .......................   Passed    0.01 sec

100% tests passed, 0 tests failed out of 9

Total Test time (real) =   0.35 sec
```

## Step 5
![image](https://user-images.githubusercontent.com/48782723/153731614-a960a258-e036-4028-8090-d40a3165a74e.png)

**CMakeLists.txt**


**MathFunctions/CMakeLists.txt**



 
