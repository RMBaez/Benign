<h1>Benign</h1>


<h2>Description</h2>
We will investigate host-centric logs in this challenge room to find suspicious process execution.

<b>One of the client’s IDS indicated a potentially suspicious process execution indicating one of the hosts from the HR department was compromised. Some tools related to network information gathering / scheduled tasks were executed which confirmed the suspicion. Due to limited resources, we could only pull the process execution logs with Event ID: 4688 and ingested them into Splunk with the index win_eventlogs for further investigation.

About the Network Information

The network is divided into three logical segments. It will help in the investigation.

<b>IT Department</b>

- James
- Moin
- Katrina

<b>HR department</b>

- Haroon
- Chris
- Diana

<b>Marketing department</b>

- Bell
- Amelia
- Deepak
  
</b>


<h2>Questions</h2>

- <b>1. How many logs are ingested from the month of March, 2022?</b>
- <b>2. Imposter Alert: There seems to be an imposter account observed in the logs, what is the name of that user?</b>
- <b>3. Which user from the HR department was observed to be running scheduled tasks?</b>
- <b>4. Which user from the HR department executed a system process (LOLBIN) to download a payload from a file-sharing host.</b>
- <b>5. To bypass the security controls, which system process (lolbin) was used to download a payload from the internet?.</b></b>
- <b>6. What was the date that this binary was executed by the infected host? format (YYYY-MM-DD)</b>
- <b>7. Which third-party site was accessed to download the malicious payload?</b>
- <b>8. What is the name of the file that was saved on the host machine from the C2 server during the post-exploitation phase?</b>
- <b>9. The suspicious file downloaded from the C2 server contained malicious content with the pattern THM{..........}; what is that pattern?</b>
- <b>10. What is the URL that the infected host connected to?</b>



<h2>Languages and Utilities Used</h2>

- <b>Splunk</b> 


<h2>Environments Used </h2>

- <b>TryHackMe VM</b>






<h2>Program walk-through</h2>

<b>Answer the question below <br/>

1. How many logs are ingested from the month of March, 2022?

<p align="center">
  First, I input into the search bar index=win-eventlogs since that is where all the information is stored and I also added EventID=4688 for the type of event discovered. Seecond, I adjusted the date range since it is asking about the month of March in the year 2022. After those two steps, I received the answer.
  <img width="1440" alt="Screenshot 2025-04-19 at 8 02 11 AM" src="https://github.com/user-attachments/assets/2aa52a86-687c-472a-aff1-7ec2cd73bcc6" />



<br />
<br />
Answer: 13959 <br/>





<h2>Program walk-through</h2>

<b>Answer the question below <br/>
2. Imposter Alert: There seems to be an imposter account observed in the logs, what is the name of that user?

<p align="center">
    I had to do a google search on how to write the proper query for creating a list of users. I was about to find it in the splunk community website. I pressed enter of the completed search query(#1) and saw two users that appeared very similar. With a closer look, I spotted the imposter(#2).
<img width="1440" alt="Screenshot 2025-04-19 at 8 23 18 AM" src="https://github.com/user-attachments/assets/45c7ad00-c41a-46a0-9b7a-79718a8efdc3" />





<br />
<br />
Answer: Amel1a <br/>




<h2>Program walk-through</h2>

<b>Answer the question below <br/>
3. Which user from the HR department was observed to be running scheduled tasks?


<p align="center">
    I didn't know how to find ran scheduled tasks on Splunk so I began to type scheduled task on the search bar and 'schtasks' appeared, I pressed enter and got results. The full search query was 'index=win-eventlogs schtasks'. I then clicked on the field UserName and saw several values. I looked at each user and only one of them where from the HR department.
<img width="1440" alt="Screenshot 2025-04-19 at 9 00 09 AM" src="https://github.com/user-attachments/assets/53b63351-b8b6-4555-806c-16c2b784fc5d" />





<br />
<br />
Answer: Chris.fort <br/>






<h2>Program walk-through</h2>

<b>Answer the question below <br/>
4. Which user from the HR department executed a system process (LOLBIN) to download a payload from a file-sharing host.</b>

<p align="center">
    I decided to filter for only the HR employees. Because there were so many commands, I decided to add the filter ' | stats count by CommandLine ' to make it easier to analyze. I looked at all the results that were executables and the only one that stood out to me was the first result that appeared.
  <img width="1440" alt="Screenshot 2025-04-19 at 9 58 15 AM" src="https://github.com/user-attachments/assets/1e747312-4d72-476e-8999-2d188e19d0b3" />
  There was a hint to this question and it read 'Explore lolbas-project.github.io/ to find binaries used to download payloads'. After doing researh on what a LOLBIN is, I googled 'lolbin certutil.exe' and read that attackers would use certutil.exe for exploitation. So now that I know I have the right command, I clicked and added the command into the search query. I looked for the field username and found the answer.
<img width="1440" alt="Screenshot 2025-04-19 at 10 20 38 AM" src="https://github.com/user-attachments/assets/7a39dbf6-59c6-4995-8443-1fdedd57246f" />





<br />
<br />
Answer: haroon <br/>



<h2>Program walk-through</h2>

<b>Answer the question below <br/>

5. To bypass the security controls, which system process (lolbin) was used to download a payload from the internet?

<p align="center">
    The answer was discussed in the previous question.




<br />
<br />
Answer: certutil.exe <br/>






<h2>Program walk-through</h2>

<b>Answer the question below <br/>

6. What was the date that this binary was executed by the infected host? format (YYYY-MM-DD)

<p align="center">
    Answer is part of same event as question 4.




<br />
<br />
Answer: 2022-03-04 <br/>





<h2>Program walk-through</h2>

<b>Answer the question below <br/>
7. Which third-party site was accessed to download the malicious payload?

<p align="center">
     Answer is part of same event as question 4.




<br />
<br />
Answer: controlc.com <br/>




<h2>Program walk-through</h2>

<b>Answer the question below <br/>
8. What is the name of the file that was saved on the host machine from the C2 server during the post-exploitation phase?



<p align="center">
    Answer is part of same event as question 4.





<br />
<br />
Answer: benign.exe<br/>






<h2>Program walk-through</h2>

<b>Answer the question below <br/>
9. The suspicious file downloaded from the C2 server contained malicious content with the pattern THM{..........}; what is that pattern?

<p align="center">
   I copied and pasted the url ' https[:]//controlc[.]com/e4d11035 '(in its fanged format) into the website VirusTotal. I went to the detail section and kept scrolling until the saw the pattern mentioned in the question
<img width="1440" alt="Screenshot 2025-04-19 at 10 36 13 AM" src="https://github.com/user-attachments/assets/27da7b70-1c00-417c-96c4-6a9f0a3162d4" />






<br />
<br />
Answer: THM{KJ&*H^B0}<br/>




<h2>Program walk-through</h2>

<b>Answer the question below <br/>
10. What is the URL that the infected host connected to?

<p align="center">
   The answer is found in the same event as in question 4.






<br />
<br />
Answer: https://controlc.com/e4d11035 <br/>

