# Lab Task 1

```jsx
Name: Hamza Haroon
Course: CY243L - Penetration Testing - Lab
Date: 13/10/2023
TotalQuestions: 5
AttemptedQuestions: 5
```

### 1. Create a new user called test

`sudo useradd test`

![Untitled 1](https://github.com/thegr1ffyn/CY243-L/assets/95119705/36028f1e-dd4b-4826-87f0-146587cede17)

### 2. Create a new group called testgrp

`sudo groupadd testgrp`

![Untitled 2](https://github.com/thegr1ffyn/CY243-L/assets/95119705/77e874f1-0530-4137-88ee-96da93d37d86)

### 3. Add the test user to the testgrp group

`sudo usermod -aG testgrp test`
![Untitled 3](https://github.com/thegr1ffyn/CY243-L/assets/95119705/59a416c9-9672-4037-a879-9f32ee0b13d6)


### 4. Create 2 new files called test1.txt and test2.txt inside the /home/kali directory and them delete them.

`touch /home/kali/test1.txt && touch /home/kali/test2.txt`
![Untitled 4](https://github.com/thegr1ffyn/CY243-L/assets/95119705/2b380d2c-e499-40c9-84fc-e6c987ae9479)


`rm /home/kali/test1.txt && rm /home/kali/test2.txt`

![Untitled 5](https://github.com/thegr1ffyn/CY243-L/assets/95119705/578d4a40-a448-43bb-9270-f01335f6bbda)

### Create a new file called my-file . Write Hello World! into the file. Change the group of this file to testgrp and give it the following permissions

a. User can read and write into the file
b. Groups can read and execute the file
c. Others can read from the file
Do this using both naming and numbers conventions.

![Untitled 6](https://github.com/thegr1ffyn/CY243-L/assets/95119705/25188934-aabd-47b1-b5df-5c15da6dff44)

Using Naming Convention
![Untitled](https://github.com/thegr1ffyn/CY243-L/assets/95119705/f08de45e-a78f-4362-8674-76626d05308e)

Using Numbered Convention
