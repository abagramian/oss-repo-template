Lab 07/08? - Testing
Alexis Bagramian


# Checkpoint 01

## Successful build
![image](https://user-images.githubusercontent.com/48782723/159775329-efe6665f-ae37-48bb-b39f-831e616ad1b7.png)

# Checkpoint 02

* Find the Nightly and Experimental sections and look at some of the submissions. How can you see what tests were run for a particular submission?

Nightly Expected: The number of tests passed and failed can be seen from the table shown on the dashboard. Clicking the number under "passed", "failed" or "not run" will show you the tests in that category. 
 

* Find a submission with errors. Can you see what the error condition was? How does this help you debug the failure?

The below screenshot shows an error found in a submission with error details. This shows you what test failed and what output it failed with, which helps to debug. 

![image](https://user-images.githubusercontent.com/48782723/160250160-6a5119e9-483a-4cfc-b93f-f04c8197c944.png)


* Find a system that is close to your specific configuration in the Nightly, Nightly Expected or one of the Masters sections. How clean is the dashboard? Are there any errors that you need to be concerned with?

The below screenshot is a test that failed on a confguration close to mine. 

![image](https://user-images.githubusercontent.com/48782723/160250231-94bab3a6-58a5-488a-8104-32852215638a.png)

Only one test failed, and the error reported that output was:

```
null
null
```

instead of

```
null
null
```

so it is not too concerning. 



## Testing Results
![image](https://user-images.githubusercontent.com/48782723/159825407-4e51130d-a222-4543-b9cd-73d51f8e230d.png)

# Checkpoint 03

## Errored Run
![image](https://user-images.githubusercontent.com/48782723/159970144-9e20fba9-9e41-4208-8e19-816127794d9a.png)

## Error Fix
Changing year from 2020 to 2022

![image](https://user-images.githubusercontent.com/48782723/159970937-4420c23d-8f47-4971-ba72-42baa87b3217.png)

## Successful Run

![image](https://user-images.githubusercontent.com/48782723/159973466-401e8079-9c76-49cc-9208-5c0025f1c206.png)


# Checkpoint 04

Repository: https://github.com/abagramian/OSSLab7/tree/master

## Successful build on Github action
![image](https://user-images.githubusercontent.com/48782723/160249321-46689dd4-5b3d-41cd-942a-c56a3973c5d1.png)

## Check passes on Pull Request creation
![image](https://user-images.githubusercontent.com/48782723/160249634-555eb4ba-d17f-46ef-846a-c497ca4ff1b9.png)

## Successful push executions
![image](https://user-images.githubusercontent.com/48782723/160249728-a80ccbd0-7f91-4554-a744-6622d00fd01a.png)


