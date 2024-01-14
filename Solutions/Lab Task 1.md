# Lab Task 1

```jsx
Name: Hamza Haroon
RollNumber: 211064
Course: CY243L - Penetration Testing - Lab
Date: 13/10/2023
TotalQuestions: 5
AttemptedQuestions: 5
```

### 1. Create a new user called test

`sudo useradd test`

![Untitled](Lab%20Task%201%20d23c549094864d5fba4be59f24c1907a/Untitled.png)

### 2. Create a new group called testgrp

`sudo groupadd testgrp`

![Untitled](Lab%20Task%201%20d23c549094864d5fba4be59f24c1907a/Untitled%201.png)

### 3. Add the test user to the testgrp group

`sudo usermod -aG testgrp test`

![Untitled](Lab%20Task%201%20d23c549094864d5fba4be59f24c1907a/Untitled%202.png)

### 4. Create 2 new files called test1.txt and test2.txt inside the /home/kali directory and them delete them.

`touch /home/kali/test1.txt && touch /home/kali/test2.txt`

![Untitled](Lab%20Task%201%20d23c549094864d5fba4be59f24c1907a/Untitled%203.png)

`rm /home/kali/test1.txt && rm /home/kali/test2.txt`

![Untitled](Lab%20Task%201%20d23c549094864d5fba4be59f24c1907a/Untitled%204.png)

### Create a new file called my-file . Write Hello World! into the file. Change the group of this file to testgrp and give it the following permissions

a. User can read and write into the file
b. Groups can read and execute the file
c. Others can read from the file
Do this using both naming and numbers conventions.

![Using Naming Convention](Lab%20Task%201%20d23c549094864d5fba4be59f24c1907a/Untitled%205.png)

Using Naming Convention

![Using Numbered Convention](Lab%20Task%201%20d23c549094864d5fba4be59f24c1907a/Untitled%206.png)

Using Numbered Convention