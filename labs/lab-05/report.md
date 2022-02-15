# Part 1

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
// A simple program that computes the square root of a number
#include <cmath>
#include <iostream>
#include <string>

#include "TutorialConfig.h"

#ifdef USE_MYMATH
#	include "MathFunctions.h"
#endif

int main(int argc, char* argv[])
{
  if (argc < 2) {
    // report version
    std::cout << argv[0] << " Version " << Tutorial_VERSION_MAJOR << "."
              << Tutorial_VERSION_MINOR << std::endl;
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
  }

  // convert input to double
  const double inputValue = std::stod(argv[1]);

  // calculate square root
  #ifdef USE_MYMATH
  	const double outputValue = mysqrt(inputValue);
  #else 
  	const double outputValue = sqrt(inputValue);
  #endif
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
  return 0;
}

```


**CMakeLists.txt**

```
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

if(USE_MYMATH)
	add_subdirectory(MathFunctions)
	list(APPEND EXTRA_LIBS MathFunctions)
	list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
			   ${EXTRA_INCLUDES}
                           )
```


## Step 3

![image](https://user-images.githubusercontent.com/48782723/153729912-c3f2c220-830b-4827-8dbf-2e5c0c5168d6.png)


**CMakeLists.txt**

```
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
  list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           ${EXTRA_INCLUDES}
                           )

```

**MathFunctions/CMakeLists.txt**

```
add_library(MathFunctions mysqrt.cxx)

target_include_directories(MathFunctions
	INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
	)
```


## Step 4

**CMakeLists.txt**

```
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )

install(TARGETS Tutorial DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
	DESTINATION include
	)

enable_testing()

add_test(NAME Runs COMMAND Tutorial 25)

add_test(NAME Usage COMMAND Tutorial)
set_tests_properties(Usage
	PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number"
	)

function(do_test target arg result)
	add_test(NAME Comp${arg} COMMAND ${target} ${arg})
	set_tests_properties(Comp${arg}
		PROPERTIES PASS_REGULAR_EXPRESSION ${result}
		)
endfunction()

do_test(Tutorial 4 "4 is 2")
do_test(Tutorial 9 "9 is 3")
do_test(Tutorial 5 "5 is 2.236")
do_test(Tutorial 7 "7 is 2.645")
do_test(Tutorial 25 "25 is 5")
do_test(Tutorial -25 "-25 is (-nan|nan|0)")
do_test(Tutorial 0.0001 "0.0001 is 0.01")

```

**MathFunction/CMakeLists.txt**

```
add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )

install(TARGETS MathFunctions DESTINATION lib)
install (FILES MathFunctions.h DESTINATION include) 

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

```
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )

# add the install targets
install(TARGETS Tutorial DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
  DESTINATION include
  )

# enable testing
enable_testing()

# does the application run
add_test(NAME Runs COMMAND Tutorial 25)

# does the usage message work?
add_test(NAME Usage COMMAND Tutorial)
set_tests_properties(Usage
  PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number"
  )

# define a function to simplify adding tests
function(do_test target arg result)
  add_test(NAME Comp${arg} COMMAND ${target} ${arg})
  set_tests_properties(Comp${arg}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result}
    )
endfunction()

# do a bunch of result based tests
do_test(Tutorial 4 "4 is 2")
do_test(Tutorial 9 "9 is 3")
do_test(Tutorial 5 "5 is 2.236")
do_test(Tutorial 7 "7 is 2.645")
do_test(Tutorial 25 "25 is 5")
do_test(Tutorial -25 "-25 is (-nan|nan|0)")
do_test(Tutorial 0.0001 "0.0001 is 0.01")


```

**MathFunctions/CMakeLists.txt**

```
add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )

include(CheckSymbolExists)
check_symbol_exists(log "math.h" HAVE_LOG)
check_symbol_exists(exp "math.h" HAVE_EXP)
if(NOT (HAVE_LOG AND HAVE_EXP))
       	unset(HAVE_LOG CACHE)
	unset(HAVE_EXP CACHE)
	set(CMAKE_REQUIRED_LIBRARIES "m")
	    check_symbol_exists(log "math.h" HAVE_LOG)
	    check_symbol_exists(exp "math.h" HAVE_EXP)
		    if(HAVE_LOG AND HAVE_EXP)
			        target_link_libraries(MathFunctions PRIVATE m)
				  endif()
			  endif()

if(HAVE_LOG AND HAVE_EXP)
	target_compile_definitions(MathFunctions
				   PRIVATE "HAVE_LOG" "HAVE_EXP")
endif()



# install rules
install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)



```


# Part 2

## make
**Makefile**
```
all: program static_block dynamic_block

clean:
	rm -f *.o *.a *.so static_block dynamic_block

program: program.o static_lib.a shared_lib.so
        cc program.o static_lib.a -o static_block
        cc program.o shared_lib.so -o dynamic_block -Wl,-rpath='$$ORIGIN'

program.o: program.c
        cc -c program.c -o program.o

block.o:  source/block.c headers/block.h
        cc -c source/block.c -o source/block.o

static_lib.a: block.o
        ar qc static_lib.a source/block.o

shared_lib.so: block.o
        cc -shared -o shared_lib.so source/block.o
```

**Output**
![image](https://user-images.githubusercontent.com/48782723/153982345-19a88317-1acd-4b1e-8d07-e71bf4b4c720.png)

**Sizes**
![image](https://user-images.githubusercontent.com/48782723/153984044-28ed7c1b-56a3-4341-bc44-ca6b9798bc37.png)


## CMake

**CMakeLists.txt**
```
cmake_minimum_required(VERSION 3.0)

# set the project name and version
project(Lab5_CMake VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_library(static_lib STATIC source/block.c)
add_library(shared_lib SHARED source/block.c)

add_executable(static_block program.c)
add_executable(dynamic_block program.c)

target_link_libraries(static_block static_lib)
target_link_libraries(dynamic_block shared_lib)
```

**Output**
![image](https://user-images.githubusercontent.com/48782723/153986277-0dba08d9-1a59-48f4-b971-28eeb1d19214.png)

**Sizes**
![image](https://user-images.githubusercontent.com/48782723/153986323-bb24cd69-2f6e-4718-bbfe-d306cb92c589.png)





 
