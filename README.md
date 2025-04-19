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

- <b>How many logs are ingested from the month of March, 2022?</b>
- <b>Imposter Alert: There seems to be an imposter account observed in the logs, what is the name of that user?</b>
- <b>Which user from the HR department was observed to be running scheduled tasks?</b>
- <b>Which user from the HR department executed a system process (LOLBIN) to download a payload from a file-sharing host.</b>
- <b>To bypass the security controls, which system process (lolbin) was used to download a payload from the internet?</b>
- <b>What was the date that this binary was executed by the infected host? format (YYYY-MM-DD)</b>
- <b>Which third-party site was accessed to download the malicious payload?</b>
- <b>What is the name of the file that was saved on the host machine from the C2 server during the post-exploitation phase?</b>
- <b>The suspicious file downloaded from the C2 server contained malicious content with the pattern THM{..........}; what is that pattern?</b>
- <b>What is the URL that the infected host connected to?</b>



<h2>Languages and Utilities Used</h2>

- <b>Splunk</b> 


<h2>Environments Used </h2>

- <b>TryHackMe VM</b>






<h2>Program walk-through</h2>

<b>Answer the question below <br/>

How many logs are ingested from the month of March, 2022?

<p align="center">
  First, I input into the search bar index=win-eventlogs since that is where all the information is stored and I also added EventID=4688 for the type of event discovered. Seecond, I adjusted the date range since it is asking about the month of March in the year 2022. After those two steps, I received the answer.
  <img width="1440" alt="Screenshot 2025-04-19 at 8 02 11 AM" src="https://github.com/user-attachments/assets/2aa52a86-687c-472a-aff1-7ec2cd73bcc6" />



<br />
<br />
Answer: 13959 <br/>





<h2>Program walk-through</h2>

<b>Answer the question below <br/>
1

<p align="center">
    The answer was discussed during question 1



<br />
<br />
Answer is EventCode=11 <br/>




<h2>Program walk-through</h2>

<b>Answer the question below <br/>
1


<p align="center">
    In the searh bar I inputted ' Image="C:\Users\Administrator\Documents\cmd.exe" md5 '. Only one event appeared with the MD5 hash value highlight
<img width="1440" alt="Screenshot 2025-04-18 at 12 41 55 PM" src="https://github.com/user-attachments/assets/bd8e94df-f2eb-469f-b987-9d2cc8202d73" />




<br />
<br />
Answer is 290c7dfb01e50cea9e19da81a781af2c <br/>






<h2>Program walk-through</h2>

<b>Answer the question below <br/>
1

<p align="center">
    I left ' Image="C:\Users\Administrator\Documents\cmd.exe" ' on the search bar but removed 'md5'.
<img width="1440" alt="Screenshot 2025-04-18 at 1 03 20 PM" src="https://github.com/user-attachments/assets/52ab777e-c0d1-4168-b926-2b1c051e93e0" />
    I scrolled down and looked through the filed and saw only named 'TargetFilename'. I looked through it and saw that the was a single file on all the results. That file was the answer to the question.
<img width="1440" alt="Screenshot 2025-04-18 at 1 03 58 PM" src="https://github.com/user-attachments/assets/a5333ef9-89a6-4de5-bf55-011089616b16" />




<br />
<br />
Answer is readme.txt <br/>



<h2>Program walk-through</h2>

<b>Answer the question below <br/>

1

<p align="center">
    On my first attempt, I put 'EventCode=4720'. I was able to see the new username created but I wasn't able to find the command for the user creation. So I went on google and searched, 'command to add new user' and the result was 'net user username password /add'. I decided only to add the '/add' onto the search bar in Splunk.
<img width="1440" alt="Screenshot 2025-04-18 at 1 24 04 PM" src="https://github.com/user-attachments/assets/2fee5fb0-9a13-493b-8d2a-e61f3b3f6e59" />
    I looked into the field named 'CommandLine' and found the new user I saw when I had ented EventCode=4720 and the command for its creation.
<img width="1440" alt="Screenshot 2025-04-18 at 1 25 06 PM" src="https://github.com/user-attachments/assets/df98e23a-1522-4953-b786-f4ff03dfe344" />




<br />
<br />
Answer is net user /add securityninja hardToHack123$ <br/>






<h2>Program walk-through</h2>

<b>Answer the question below <br/>

1

<p align="center">
    There was a hint to this question. The hint was ' Try sysmon event code 8 '. I did not know what event code 8 was so I did a quick google seach and this is what I received.
<img width="900" alt="image" src="https://github.com/user-attachments/assets/b194a710-dc5c-4947-9d73-5a95ae917a12" />
    I put EventCode=8 into the search bar in Splunk and received two events.
<img width="1440" alt="image" src="https://github.com/user-attachments/assets/64a232d5-29c4-4972-8172-52bc0f938b38" />
    I went down looking in the interesting fields section and under the field 'SourceImage' was the answer.
<img width="1440" alt="Screenshot 2025-04-18 at 2 48 34 PM" src="https://github.com/user-attachments/assets/ad4241fb-d4de-4f17-857b-7c12a1233a4a" />





<br />
<br />
Answer is C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe,C:\Windows\System32\wbem\unsecapp.exe <br/>





<h2>Program walk-through</h2>

<b>Answer the question below <br/>
1

<p align="center">
    In this question, instead of the field SourceImage, I went to TargetImage. I selected the process migation that wasn't used for the previous question for this question and it was the right answer.
<img width="1440" alt="Screenshot 2025-04-18 at 3 06 46 PM" src="https://github.com/user-attachments/assets/09b0f8c3-2cc4-424f-b9ff-b804c1babd99" />




<br />
<br />
Answer is C:\Windows\System32\lsass.exe <br/>




<h2>Program walk-through</h2>

<b>Answer the question below <br/>
1


<p align="center">
    To be honest, I did not know what I was looking for. The hint I received for this question was 'Try looking in the IIS logs for POST requests.' I did not know what IIS logs are but I have an idea of what POST requests are. So in the Splunk search bar I added POST and pressed enter. I started searching through the interested fields looking for 'IIS'. In the field sourcetype, it had 'iis' as a value so I added that into the search bar as well.
<img width="1440" alt="image" src="https://github.com/user-attachments/assets/d93e3f9d-a8fe-4b9c-aa08-18cd2d2d296a" />
    I don't know what a web shell exploit looks like so I googled 'web shell file type exploit'. While do some reading, I came across several and inputting them into the search bar in Splunk with the wildcard(*) attached hoping for a bite. The only one that produced results was 'asp'.
<img width="1440" alt="image" src="https://github.com/user-attachments/assets/4bfe7d2f-1bd6-46a6-bedb-1336e6190cef" />
    I saw a field named 'cs_uri_stem'. Although I don't know much of URIs, but I know it relates to webpages. I looked through its values and found the answer.
<img width="1440" alt="Screenshot 2025-04-18 at 4 09 00 PM" src="https://github.com/user-attachments/assets/35b36219-b831-4060-a0af-1a1d87f0360a" />





<br />
<br />
Answer is i3gfPctK1c2x.aspx <br/>






<h2>Program walk-through</h2>

<b>Answer the question below <br/>
1

<p align="center">
   In the search car I put i3gfPctK1c2x.aspx. One event appeared. I looked at the field CommandLine and found the answer.
<img width="1440" alt="Screenshot 2025-04-18 at 4 15 21 PM" src="https://github.com/user-attachments/assets/1c507042-c46a-46c4-b8c4-6095f998fc16" />





<br />
<br />
Answer: attrib.exe -r \\\\win-aoqkg2as2q7.bellybear.local\C$\Program Files\Microsoft\Exchange Server\V15\FrontEnd\HttpProxy\owa\auth\i3gfPctK1c2x.aspx <br/>





